---
title: 將 WCF 雙工服務遷移至 WCF 開發人員的 gRPC-gRPC
description: 瞭解如何將各種形式的 WCF 雙工服務遷移至 gRPC 串流服務。
author: markrendle
ms.date: 09/02/2019
ms.openlocfilehash: 06ac784a31df43fd270f7ef0475bcdc282efad8f
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2019
ms.locfileid: "71184334"
---
# <a name="migrate-wcf-duplex-services-to-grpc"></a>將 WCF 雙工服務遷移至 gRPC

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

現在基本概念已就緒，本節將探討更複雜的*串流*gRPC 服務。

有多種方式可以在 Windows Communication Foundation （WCF）中使用雙工服務。 某些服務是由用戶端起始，然後從伺服器串流資料。 其他全雙工服務可能牽涉到更多進行中的雙向通訊，例如 WCF 檔中的傳統「計算機」範例。 本章將採用兩個可能的 WCF 「股票行情指示器」，並將其遷移至 gRPC：一個使用伺服器串流 RPC，另一個使用雙向串流 RPC。

## <a name="server-streaming-rpc"></a>伺服器串流 RPC

在[範例 SIMPLESTOCKTICKER WCF 方案](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/SimpleStockTickerSample/wcf/SimpleStockTicker) *SimpleStockPriceTicker*中，有一個雙工服務，其中用戶端會使用內建符號清單來啟動連線，而伺服器會使用*回呼介面*來傳送更新可供使用。 用戶端會執行該介面，以回應來自伺服器的呼叫。

### <a name="the-wcf-solution"></a>WCF 解決方案

在 .NET Framework 4.x 主控台應用程式中，WCF 解決方案會實作為自我裝載的 NetTCP 伺服器。

#### <a name="the-servicecontract"></a>ServiceContract

```csharp
[ServiceContract(SessionMode = SessionMode.Required, CallbackContract = typeof(ISimpleStockTickerCallback))]
public interface ISimpleStockTickerService
{
    [OperationContract(IsOneWay = true)]
    void Subscribe(string[] symbols);
}
```

服務具有不具有傳回類型的單一方法，因為它會使用回呼介面`ISimpleStockTickerCallback` ，即時將資料傳送至用戶端。

#### <a name="the-callback-interface"></a>回呼介面

```csharp
[ServiceContract]
public interface ISimpleStockTickerCallback
{
    [OperationContract(IsOneWay = true)]
    void Update(string symbol, decimal price);
}
```

您可以在解決方案中找到這些介面的執行，以及偽造的外部相依性，以提供測試資料。

### <a name="grpc-streaming"></a>gRPC 串流

處理即時資料的 gRPC 方式不同。 從用戶端到伺服器的呼叫可以建立持續性資料流程，可以針對以非同步方式到達的訊息進行監視。 雖然不同，資料流程可以是更直覺的方式來處理這項資料，而且在現代化的程式設計上更具相關性，並著重于 LINQ、回應式資料流程、功能性程式設計等等。

服務定義需要兩個訊息：一個用於要求，另一個用於資料流程。 服務會在其`StockTickerUpdate` `return`宣告中使用關鍵字， `stream`以傳回訊息的資料流程。 建議您將新增`Timestamp`至更新，以顯示價格變更的確切時間。

#### <a name="simple_stock_tickerproto"></a>simple_stock_ticker. proto

```protobuf
syntax = "proto3";

option csharp_namespace = "TraderSys.SimpleStockTickerServer.Protos";

import "google/protobuf/timestamp.proto";

package SimpleStockTickerServer;

service SimpleStockTicker {
  rpc Subscribe (SubscribeRequest) returns (stream StockTickerUpdate);
}

message SubscribeRequest {
  repeated string symbols = 1;
}

message StockTickerUpdate {
  string symbol = 1;
  int32 priceCents = 2;
  google.protobuf.Timestamp time = 3;
}
```

### <a name="implement-the-simplestockticker"></a>執行 SimpleStockTicker

將`TraderSys.StockMarket`類別庫`StockPriceSubscriber`中的三個類別複製到目標方案中的新 .NET Standard 類別庫，以重複使用 WCF 專案中的假。 若要進一步遵循最佳做法，請`Factory`新增類型來建立其實例，並向`IStockPriceSubscriberFactory` ASP.NET Core 的相依性插入服務註冊。

#### <a name="the-factory-implementation"></a>Factory 的執行

```csharp
public interface IStockPriceSubscriberFactory
{
    IStockPriceSubscriber GetSubscriber(string[] symbols);
}

public class StockPriceSubscriberFactory : IStockPriceSubscriberFactory
{
    public IStockPriceSubscriber GetSubscriber(string[] symbols)
    {
        return new StockPriceSubscriber(symbols);
    }
}
```

#### <a name="registering-the-factory"></a>註冊 factory

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton<IStockPriceSubscriberFactory, StockPriceSubscriberFactory>();
}
```

現在，這個類別可以用來執行 gRPC StockTicker 服務。

#### <a name="stocktickerservicecs"></a>StockTickerService.cs

```csharp
public class StockTickerService : Protos.SimpleStockTicker.SimpleStockTickerBase
{
    private readonly IStockPriceSubscriberFactory _subscriberFactory;

    public StockTickerService(IStockPriceSubscriberFactory subscriberFactory)
    {
        _subscriberFactory = subscriberFactory;
    }

    public override async Task Subscribe(SubscribeRequest request, IServerStreamWriter<StockTickerUpdate> responseStream, ServerCallContext context)
    {
        var subscriber = _subscriberFactory.GetSubscriber(request.Symbols.ToArray());

        subscriber.Update += async (sender, args) =>
            await WriteUpdateAsync(responseStream, args.Symbol, args.Price);

        await AwaitCancellation(context.CancellationToken);
    }

    private async Task WriteUpdateAsync(IServerStreamWriter<StockTickerUpdate> stream, string symbol, decimal price)
    {
        try
        {
            await stream.WriteAsync(new StockTickerUpdate
            {
                Symbol = symbol,
                PriceCents = (int)(price * 100),
                Time = Timestamp.FromDateTimeOffset(DateTimeOffset.UtcNow)
            });
        }
        catch (Exception e)
        {
            // Handle any errors due to broken connection etc.
            _logger.LogError($"Failed to write message: {e.Message}");
        }
    }

    private static Task AwaitCancellation(CancellationToken token)
    {
        var completion = new TaskCompletionSource<object>();
        token.Register(() => completion.SetResult(null));
        return completion.Task;
    }
}
```

如您所見，雖然檔案中`.proto`的宣告指出方法會「傳回」 `StockTickerUpdate`訊息的資料流程，事實上它會傳回 vanilla `Task`。 建立資料流程的作業是由產生的程式碼和 gRPC 執行時間程式庫（可提供`IServerStreamWriter<StockTickerUpdate>`回應資料流程）來處理，以供使用。

不同于 WCF 雙工服務，在連接開啟時，服務類別的實例會保持運作狀態，而 gRPC 服務則會使用傳回的工作來保持服務的運作狀態。 工作不應完成，直到連接關閉為止。

服務可以使用`CancellationToken` `ServerCallContext`從來判斷用戶端已關閉連接的時機。 簡單的靜態方法`AwaitCancellation`是用來建立在解除標記時完成的工作。

在方法中，取得並加入事件處理常式，以寫入至回應資料流程。 `StockPriceSubscriber` `Subscribe` 然後在立即處置`subscriber`之前，等待連接關閉，以防止它嘗試將資料寫入已關閉的資料流程。

方法有一個`try` 區塊，可`catch`處理將訊息寫入資料流程時可能發生的任何錯誤。 / `WriteUpdateAsync` 這是透過網路的持續連線中的重要考慮，不論是刻意或因為發生失敗，都可以在任何毫秒中斷。

### <a name="using-the-stocktickerservice-from-a-client-application"></a>從用戶端應用程式使用 StockTickerService

遵循上一節中的相同步驟，從`.proto`檔案建立可共用的用戶端類別庫。 在此範例中，有一個 .NET Core 3.0 主控台應用程式，示範如何使用用戶端。

#### <a name="example-programcs"></a>範例 Program.cs

```csharp
class Program
{
    static async Task Main(string[] args)
    {
        using var channel = GrpcChannel.ForAddress("https://localhost:5001");
        var client = new SimpleStockTicker.SimpleStockTickerClient(channel);

        var request = new SubscribeRequest();
        request.Symbols.AddRange(args);

        using var stream = client.Subscribe(request);

        var tokenSource = new CancellationTokenSource();

        var task = DisplayAsync(stream.ResponseStream, tokenSource.Token);

        WaitForExitKey();

        tokenSource.Cancel();
        await task;
    }
}
```

在此情況下， `Subscribe`產生的用戶端上的方法不是非同步。 資料流程會立即建立並可供使用，因為它`MoveNext`的方法是非同步，而且第一次呼叫它時，必須等到連接正常運作後才會完成。

資料流程會傳遞至非同步`DisplayAsync`方法; 接著，應用程式會等待使用者按下某個按鍵，然後取消該`DisplayAsync`方法並等候工作完成後再結束。

> [!NOTE]
> 此程式碼會使用新C#的 8 "using 宣告" 語法來處置資料流程，並在`Main`方法結束時處理通道。 這是一項很小的變更，但可減少縮排和空白行的好處。

#### <a name="consume-the-stream"></a>使用資料流程

WCF 使用的回呼介面，可讓伺服器直接呼叫用戶端上的方法。 gRPC 串流的工作方式不同。 用戶端會逐一查看傳回的資料流程並處理訊息，就像是從傳回的本機方法`IEnumerable`傳回的一樣。

類型的運作方式非常`IEnumerator<T>`類似：有一個`MoveNext`方法會在有更多資料時傳回 true，而`Current`屬性則會傳回最新的值。 `IAsyncStreamReader<T>` 唯一的差別在於， `MoveNext`方法`Task<bool>`會傳回，而不是只`bool`傳回。 擴充方法會將資料流程包裝在標準C#的 8 `IAsyncEnumerable`中，以用於新`await foreach`的語法。 `ReadAllAsync`

```csharp
static async Task DisplayAsync(IAsyncStreamReader<StockTickerUpdate> stream, CancellationToken token)
{
    try
    {
        await foreach (var update in stream.ReadAllAsync(token))
        {
            Console.WriteLine($"{update.Symbol}: {update.Price}");
        }
    }
    catch (RpcException e) when (e.StatusCode == StatusCode.Cancelled)
    {
        return;
    }
    catch (OperationCanceledException)
    {
        Console.WriteLine("Finished.");
    }
}
```

> [!TIP]
> 本章節結尾的[用戶端程式庫](client-libraries.md#iobservable)一節將探討如何新增擴充方法和類別，以包裝`IAsyncStreamReader<T>`在中，讓開發人員使用回應式的程式`IObservable<T>`設計模式。

同樣地，請小心在此處攔截例外狀況<xref:System.OperationCanceledException> ，因為可能發生網路失敗，以及可避免擲回的，因為程式碼<xref:System.Threading.CancellationToken>使用來中斷迴圈。 型別有許多關於 gRPC 執行時間錯誤的實用資訊， `StatusCode`包括。 `RpcException` 如需詳細資訊，請參閱[第4章中的*錯誤處理*](error-handling.md)

## <a name="bidirectional-streaming"></a>雙向串流

WCF 全雙工服務可雙向進行非同步即時訊息傳遞。 在伺服器串流範例中，用戶端會啟動要求，然後接收更新的串流。 該服務的較佳版本可讓用戶端在清單中新增和移除股票，而不需要停止和建立新的訂用帳戶。 該功能已在[FullStockTicker 範例解決方案](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/FullStockTickerSample/wcf/FullStockTicker)中執行。

`IFullStockTickerService`介面提供三種方法：

- `Subscribe`啟動連接。
- `AddSymbol`新增要監看的股票符號。
- `RemoveSymbol`從監看清單中移除符號。

```csharp
[ServiceContract(SessionMode = SessionMode.Required, CallbackContract = typeof(IFullStockTickerCallback))]
public interface IFullStockTickerService
{
    [OperationContract(IsOneWay = true)]
    void Subscribe();

    [OperationContract(IsOneWay = true)]
    void AddSymbol(string symbol);

    [OperationContract(IsOneWay = true)]
    void RemoveSymbol(string symbol);
}
```

回呼介面保持不變。

在 gRPC 中執行此模式較不簡單，因為現在有兩個數據串流會傳遞訊息：一個是從用戶端到伺服器，另一個則是從伺服器到用戶端。 您無法使用多個方法來執行新增和移除作業，但是可以在單一資料流程上傳遞多個類型的訊息，方法是使用[第3章](protobuf-any-oneof.md)所`Any`涵蓋的`oneof`類型或關鍵字。

在有一組可接受的特定類型的情況下， `oneof`是較好的方式。 使用可以保存`AddSymbolRequest` 或`RemoveSymbolRequest`的。 `ActionMessage`

```protobuf
message ActionMessage {
  oneof action {
    AddSymbolRequest add = 1;
    RemoveSymbolRequest remove = 2;
  }
}

message AddSymbolRequest {
  string symbol = 1;
}

message RemoveSymbolRequest {
  string symbol = 1;
}
```

宣告採用`ActionMessage`訊息資料流程的雙向串流服務。

```protobuf
service FullStockTicker {
  rpc Subscribe (stream ActionMessage) returns (stream StockTickerUpdate);
}
```

這項服務的實作為與上一個範例`Subscribe`類似，但方法的第一個參數`IAsyncStreamReader<ActionMessage>`現在是，它可以用來處理`Add`和`Remove`要求。

```csharp
public override async Task Subscribe(IAsyncStreamReader<ActionMessage> requestStream, IServerStreamWriter<StockTickerUpdate> responseStream, ServerCallContext context)
{
    using var subscriber = _subscriberFactory.GetSubscriber();

    subscriber.Update += async (sender, args) =>
        await WriteUpdateAsync(responseStream, args.Symbol, args.Price);

    var actionsTask = HandleActions(requestStream, subscriber, context.CancellationToken);

    _logger.LogInformation("Subscription started.");
    await AwaitCancellation(context.CancellationToken);

    try { await actionsTask; } catch { /* Ignored */ }

    _logger.LogInformation("Subscription finished.");
}

private async Task WriteUpdateAsync(IServerStreamWriter<StockTickerUpdate> stream, string symbol, decimal price)
{
    try
    {
        await stream.WriteAsync(new StockTickerUpdate
        {
            Symbol = symbol,
            PriceCents = (int)(price * 100),
            Time = Timestamp.FromDateTimeOffset(DateTimeOffset.UtcNow)
        });
    }
    catch (Exception e)
    {
        // Handle any errors due to broken connection etc.
        _logger.LogError($"Failed to write message: {e.Message}");
    }
}

private static Task AwaitCancellation(CancellationToken token)
{
    var completion = new TaskCompletionSource<object>();
    token.Register(() => completion.SetResult(null));
    return completion.Task;
}
```

GRPC 為我們產生的`Add` `Remove` `null`類別可確保只有其中一個和屬性可以設定，並找出哪一個不是有效的方法來尋找所使用的訊息類型，但還有更好的方法`ActionMessage`. 程式`enum ActionOneOfCase`代碼產生也會`ActionMessage`在類別中建立，如下所示：

```csharp
public enum ActionOneofCase {
    None = 0,
    Add = 1,
    Remove = 2,
}
```

物件上的`ActionCase` 屬性`switch`可以與語句搭配使用，以決定要設定的欄位。 `ActionMessage`

```csharp
private async Task HandleActions(IAsyncStreamReader<ActionMessage> requestStream, IFullStockPriceSubscriber subscriber, CancellationToken token)
{
    await foreach (var action in requestStream.ReadAllAsync(token))
    {
        switch (action.ActionCase)
        {
            case ActionMessage.ActionOneofCase.None:
                _logger.LogWarning("No Action specified.");
                break;
            case ActionMessage.ActionOneofCase.Add:
                subscriber.Add(action.Add.Symbol);
                break;
            case ActionMessage.ActionOneofCase.Remove:
                subscriber.Remove(action.Remove.Symbol);
                break;
            default:
                _logger.LogWarning($"Unknown Action '{action.ActionCase}'.");
                break;
        }
    }
}
```

> [!TIP]
> 語句有一個`default`案例，會在遇到未知`ActionOneOfCase`的值時記錄警告。 `switch` 這在表示用戶端正在使用較新版本`.proto`的檔案，而該檔案已新增更多動作時很有用。 這是使用的`switch`原因之一，比`null`在已知的欄位上測試更好。

### <a name="use-the-fullstocktickerservice-from-a-client-application"></a>從用戶端應用程式使用 FullStockTickerService

有一個簡單的 .NET Core 3.0 WPF 應用程式，示範如何使用這個更複雜的用戶端。 您可以[在 GitHub 上](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/FullStockTickerSample/grpc/FullStockTicker)找到完整的應用程式。

用戶端是在`MainWindowViewModel`類別中使用，它會從相依性插入取得`FullStockTicker.FullStockTickerClient`類型的實例。

```csharp
public class MainWindowViewModel : IAsyncDisposable, INotifyPropertyChanged
{
    private readonly FullStockTicker.FullStockTickerClient _client;
    private readonly AsyncDuplexStreamingCall<ActionMessage, StockTickerUpdate> _duplexStream;
    private readonly CancellationTokenSource _cancellationTokenSource;
    private readonly Task _responseTask;
    private string _addSymbol;

    public MainWindowViewModel(FullStockTicker.FullStockTickerClient client)
    {
        _cancellationTokenSource = new CancellationTokenSource();
        _client = client;
        _duplexStream = _client.Subscribe();
        _responseTask = HandleResponsesAsync(_cancellationTokenSource.Token);

        AddCommand = new AsyncCommand(Add, CanAdd);
    }
```

`client.Subscribe()`方法所傳回的物件現在是 gRPC 程式庫類型`AsyncDuplexStreamingCall<TRequest, TResponse>`的實例，它會提供`RequestStream`將要求傳送至伺服器的，以及`ResponseStream`用於處理回應的。

從某些 WPF `ICommand`方法使用要求資料流程來加入和移除符號。 針對每個作業，設定`ActionMessage`物件的相關欄位：

```csharp
private async Task Add()
{
    if (CanAdd())
    {
        await _duplexStream.RequestStream.WriteAsync(new ActionMessage {Add = new AddSymbolRequest {Symbol = AddSymbol}});
    }
}

public async Task Remove(PriceViewModel priceViewModel)
{
    await _duplexStream.RequestStream.WriteAsync(new ActionMessage {Remove = new RemoveSymbolRequest {Symbol = priceViewModel.Symbol}});
    Prices.Remove(priceViewModel);
}
```

> [!IMPORTANT]
> 在訊息上設定欄位的值會自動清除先前已設定的所有欄位。`oneof`

回應的資料流程會在`async`方法中處理，而傳回的`Task`則會在視窗關閉時保持處置。

```csharp
private async Task HandleResponsesAsync(CancellationToken token)
{
    var stream = _duplexStream.ResponseStream;

    try
    {
        await foreach (var update in stream.ReadAllAsync(token))
        {
            var price = Prices.FirstOrDefault(p => p.Symbol.Equals(update.Symbol));
            if (price == null)
            {
                price = new PriceViewModel(this) {Symbol = update.Symbol, Price = update.PriceCents / 100m};
                Prices.Add(price);
            }
            else
            {
                price.Price = update.PriceCents / 100m;
            }
        }
    }
    catch (OperationCancelledException) { }
}
```

### <a name="client-clean-up"></a>用戶端清理

當視窗關閉`MainWindowViewModel`並處置（從的`Closed`事件`MainWindow`）時，建議您適當地處置`AsyncDuplexStreamingCall`物件。 特別的是， `CompleteAsync` `RequestStream`應該呼叫上的方法，以正常關閉伺服器上的資料流程。 下列範例顯示範例視圖`DisposeAsync`模型中的方法：

```csharp
public ValueTask DisposeAsync()
{
    try
    {
        await _duplexStream.RequestStream.CompleteAsync().ConfigureAwait(false);
        await _responseTask.ConfigureAwait(false);
    }
    finally
    {
        _duplexStream.Dispose();
    }
}
```

關閉要求資料流程可讓伺服器及時處置自己的資源。 這可以改善服務的效率和擴充性，並避免發生例外狀況。

>[!div class="step-by-step"]
>[上一頁](migrate-request-reply.md)
>[下一頁](streaming-versus-repeated.md)
