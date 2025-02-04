---
title: 處理非同步應用程式中的重新進入（Visual Basic）
ms.date: 07/20/2015
ms.assetid: ef3dc73d-13fb-4c5f-a686-6b84148bbffe
ms.openlocfilehash: 199b7ce2cb8b3f3b8e220f9e2bab7e9c39a8d033
ms.sourcegitcommit: da2dd2772fcf32b44eb18b1cbe8affd17b1753c9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71351983"
---
# <a name="handling-reentrancy-in-async-apps-visual-basic"></a>處理非同步應用程式中的重新進入（Visual Basic）

當您將非同步程式碼納入您的應用程式時，應該考慮並防止可能發生的重新進入，也就是在完成前重新進入的非同步作業。 如果您不找出並處理重新進入的可能性，它可能會導致非預期的結果。

> [!NOTE]
> 若要執行範例，您必須在電腦上安裝 Visual Studio 2012 或更新版本以及 .NET Framework 4.5 或更新版本。

## <a name="BKMK_RecognizingReentrancy"></a> 辨識重新進入

在本主題的範例中，使用者選擇 [開始] 按鈕來起始非同步應用程式，該應用程式會下載一系列網站，並計算下載的位元組總數。 此範例的同步版本會回應相同的方式，不論使用者選擇按鈕的次數為何，因為在第一次之後，UI 執行緒會忽略這些事件，直到應用程式完成執行為止。 但在非同步應用程式中，UI 執行緒會繼續回應，而且您可能在它完成之前重新進入非同步作業。

如果使用者只選擇一次 [開始] 按鈕，則下列範例會顯示預期的輸出。 下載的網站清單會顯示每個網站的大小 (以位元組為單位)。 結尾會出現位元組總數。

```console
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
5. msdn.microsoft.com/library/hh524395.aspx                68959
6. msdn.microsoft.com/library/ms404677.aspx               197325
7. msdn.microsoft.com                                            42972
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591
```

不過，如果使用者選擇按鈕多次，則會重複叫用事件處理常式，且每次都會重新進入下載程序。 如此一來，同時會執行數個非同步作業，輸出會與結果交錯，且導出令人困惑的位元組總數。

```console
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
5. msdn.microsoft.com/library/hh524395.aspx                68959
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
6. msdn.microsoft.com/library/ms404677.aspx               197325
3. msdn.microsoft.com/library/jj155761.aspx                29019
7. msdn.microsoft.com                                            42972
4. msdn.microsoft.com/library/hh290140.aspx               117152
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591

5. msdn.microsoft.com/library/hh524395.aspx                68959
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
6. msdn.microsoft.com/library/ms404677.aspx               197325
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
7. msdn.microsoft.com                                            42972
5. msdn.microsoft.com/library/hh524395.aspx                68959
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591

6. msdn.microsoft.com/library/ms404677.aspx               197325
7. msdn.microsoft.com                                            42972
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591
```

您可以捲動到本主題的結尾來檢閱產生此輸出的程式碼。 您可以將方案下載到本機電腦，然後執行 WebsiteDownload 專案；或使用本主題結尾的程式碼來建立您自己的專案，以實驗程式碼。如需詳細資訊和指示，請參閱[檢閱及執行範例應用程式](#BKMD_SettingUpTheExample)。

## <a name="BKMK_HandlingReentrancy"></a>處理重新進入

您可以各種不同的方式重新進入，視您要應用程式執行的工作而定。 本主題提供下列範例：

- [停用開始按鈕](#BKMK_DisableTheStartButton)

  在執行作業時停用 [開始] 按鈕，讓使用者無法中斷它。

- [取消後再重新啟動作業](#BKMK_CancelAndRestart)

  當使用者再次選擇 [開始] 按鈕，然後讓最近要求的作業繼續進行時，取消仍在執行的任何作業。

- [執行多個作業並將輸出加入佇列](#BKMK_RunMultipleOperations)

  允許所有要求的作業以非同步方式執行，但協調輸出的顯示，以一起並循序顯示每個作業的結果。

### <a name="BKMK_DisableTheStartButton"></a> 停用 [開始] 按鈕

您可以停用 `StartButton_Click` 事件處理常式頂端的按鈕，以便在執行作業時封鎖 [開始] 按鈕。 作業完成時，您可以在 `Finally` 區塊中重新啟用按鈕，讓使用者可再次執行應用程式。

下列程式碼會顯示這些變更 (以星號標記)。 您可以將變更新增至本主題結尾的程式碼，也可以從 @no__t 0Async 範例下載已完成的應用程式：Reentrancy in .NET Desktop Apps](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06) (非同步範例︰重新進入 .NET 傳統型應用程式) 下載壓縮檔案。 專案名稱是 DisableStartButton。

```vb
Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)
    ' This line is commented out to make the results clearer in the output.
    'ResultsTextBox.Text = ""

    ' ***Disable the Start button until the downloads are complete.
    StartButton.IsEnabled = False

    Try
        Await AccessTheWebAsync()

    Catch ex As Exception
        ResultsTextBox.Text &= vbCrLf & "Downloads failed."
    ' ***Enable the Start button in case you want to run the program again.
    Finally
        StartButton.IsEnabled = True

    End Try
End Sub
```

因為變更，按鈕不會在 `AccessTheWebAsync` 正在下載網站時反應，因此程序將無法重新進入。

### <a name="BKMK_CancelAndRestart"></a> 取消後再重新啟動作業

您不必停用 [開始] 按鈕，您可以讓按鈕保持作用中，但如果使用者再次選擇該按鈕，請取消已在執行的作業，並讓最近啟動的作業繼續執行。

如需取消的詳細資訊，請參閱[微調非同步應用程式（Visual Basic）](../../../../visual-basic/programming-guide/concepts/async/fine-tuning-your-async-application.md)。

若要設定此案例，請對[檢閱及執行範例應用程式](#BKMD_SettingUpTheExample)中提供的基本程式碼進行下列變更。 您也可以從 [Async Samples:Reentrancy in .NET Desktop Apps](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06) (非同步範例︰重新進入 .NET 傳統型應用程式) 下載壓縮檔案。 此專案的名稱是 CancelAndRestart。

1. 宣告 <xref:System.Threading.CancellationTokenSource> 變數 `cts`，這是在所有方法的範圍內。

    ```vb
    Class MainWindow // Or Class MainPage

        ' *** Declare a System.Threading.CancellationTokenSource.
        Dim cts As CancellationTokenSource
    ```

2. 在 `StartButton_Click` 中，判定作業是否已在進行中。 如果 `cts` 的值為 `Nothing`，則沒有任何作業已在使用中。 如果值不 `Nothing`，則已取消已在執行中的作業。

    ```vb
    ' *** If a download process is already underway, cancel it.
    If cts IsNot Nothing Then
        cts.Cancel()
    End If
    ```

3. 將 `cts` 設為代表目前程序的不同值。

    ```vb
    ' *** Now set cts to cancel the current process if the button is chosen again.
    Dim newCTS As CancellationTokenSource = New CancellationTokenSource()
    cts = newCTS
    ```

4. 在 `StartButton_Click` 結束時，目前的進程已完成，因此請將 `cts` 的值設定回 `Nothing`。

    ```vb
    ' *** When the process completes, signal that another process can proceed.
    If cts Is newCTS Then
        cts = Nothing
    End If
    ```

下列程式碼顯示 `StartButton_Click` 中的所有變更。 新增的項目會以星號標記。

```vb
Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)

    ' This line is commented out to make the results clearer.
    'ResultsTextBox.Text = ""

    ' *** If a download process is underway, cancel it.
    If cts IsNot Nothing Then
        cts.Cancel()
    End If

    ' *** Now set cts to cancel the current process if the button is chosen again.
    Dim newCTS As CancellationTokenSource = New CancellationTokenSource()
    cts = newCTS

    Try
        ' *** Send a token to carry the message if the operation is canceled.
        Await AccessTheWebAsync(cts.Token)

    Catch ex As OperationCanceledException
        ResultsTextBox.Text &= vbCrLf & "Download canceled." & vbCrLf

    Catch ex As Exception
        ResultsTextBox.Text &= vbCrLf & "Downloads failed."
    End Try

    ' *** When the process is complete, signal that another process can proceed.
    If cts Is newCTS Then
        cts = Nothing
    End If
End Sub
```

在 `AccessTheWebAsync` 中進行下列變更。

- 加入參數以接受來自 `StartButton_Click` 的取消語彙基元。

- 使用 <xref:System.Net.Http.HttpClient.GetAsync%2A> 方法來下載網站，因為 `GetAsync` 接受 <xref:System.Threading.CancellationToken> 引數。

- 在呼叫 `DisplayResults` 顯示每個下載網站的結果前，請檢查 `ct` 以確認目前的作業尚未取消。

 下列程式碼會顯示這些變更 (以星號標記)。

```vb
' *** Provide a parameter for the CancellationToken from StartButton_Click.
Private Async Function AccessTheWebAsync(ct As CancellationToken) As Task

    ' Declare an HttpClient object.
    Dim client = New HttpClient()

    ' Make a list of web addresses.
    Dim urlList As List(Of String) = SetUpURLList()

    Dim total = 0
    Dim position = 0

    For Each url In urlList
        ' *** Use the HttpClient.GetAsync method because it accepts a
        ' cancellation token.
        Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)

        ' *** Retrieve the website contents from the HttpResponseMessage.
        Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

        ' *** Check for cancellations before displaying information about the
        ' latest site.
        ct.ThrowIfCancellationRequested()

        position += 1
        DisplayResults(url, urlContents, position)

        ' Update the total.
        total += urlContents.Length
    Next

    ' Display the total count for all of the websites.
    ResultsTextBox.Text &=
        String.Format(vbCrLf & vbCrLf & "TOTAL bytes returned:  " & total & vbCrLf)
End Function
```

如果您在此應用程式執行時，選擇 [**開始**] 按鈕多次，則應該會產生類似下列輸出的結果：

```console
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               122505
5. msdn.microsoft.com/library/hh524395.aspx                68959
6. msdn.microsoft.com/library/ms404677.aspx               197325
Download canceled.

1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
Download canceled.

1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
5. msdn.microsoft.com/library/hh524395.aspx                68959
6. msdn.microsoft.com/library/ms404677.aspx               197325
7. msdn.microsoft.com                                            42972
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591
```

若要排除部分清單，請取消註解 `StartButton_Click` 中程式碼的第一行，以清除每次使用者重新啟動作業時出現的文字方塊。

### <a name="BKMK_RunMultipleOperations"></a> 執行多個作業並將輸出加入佇列

此第三個範例是最複雜的，因為每當使用者選擇 [開始] 按鈕時，應用程式就會啟動另一個非同步作業，而且所有作業都會執行到完成為止。 所有要求的作業會以非同步方式從清單下載網站，但作業的輸出會以循序方式呈現。 也就是隨[辨識重新進入](#BKMK_RecognizingReentrancy)顯示輸出，實際的下載活動會交錯進行，但每個群組的結果清單會循序呈現。

作業會共用全域 <xref:System.Threading.Tasks.Task>，`pendingWork`，做為顯示程序的閘道管理員。

您可以執行這個範例，方法是將變更貼至[建置應用程式](#BKMK_BuildingTheApp)中的程式碼，或者遵循[下載應用程式](#BKMK_DownloadingTheApp)的指示來下載範例，然後執行 QueueResults 專案。

下列輸出顯示當使用者只選擇 [開始] 按鈕一次時的結果。 字母標籤 A，表示第一次選擇 [開始] 按鈕時的結果。 數字顯示下載目標清單中的 URL 順序。

```console
#Starting group A.
#Task assigned for group A.

A-1. msdn.microsoft.com/library/hh191443.aspx                87389
A-2. msdn.microsoft.com/library/aa578028.aspx               209858
A-3. msdn.microsoft.com/library/jj155761.aspx                30870
A-4. msdn.microsoft.com/library/hh290140.aspx               119027
A-5. msdn.microsoft.com/library/hh524395.aspx                71260
A-6. msdn.microsoft.com/library/ms404677.aspx               199186
A-7. msdn.microsoft.com                                            53266
A-8. msdn.microsoft.com/library/ff730837.aspx               148020

TOTAL bytes returned:  918876

#Group A is complete.
```

如果使用者選擇 [開始] 按鈕三次，應用程式會產生類似下列幾行的輸出。 以井字號 (#) 開頭的資訊行會追蹤應用程式的進度。

```console
#Starting group A.
#Task assigned for group A.

A-1. msdn.microsoft.com/library/hh191443.aspx                87389
A-2. msdn.microsoft.com/library/aa578028.aspx               207089
A-3. msdn.microsoft.com/library/jj155761.aspx                30870
A-4. msdn.microsoft.com/library/hh290140.aspx               119027
A-5. msdn.microsoft.com/library/hh524395.aspx                71259
A-6. msdn.microsoft.com/library/ms404677.aspx               199185

#Starting group B.
#Task assigned for group B.

A-7. msdn.microsoft.com                                            53266

#Starting group C.
#Task assigned for group C.

A-8. msdn.microsoft.com/library/ff730837.aspx               148010

TOTAL bytes returned:  916095

B-1. msdn.microsoft.com/library/hh191443.aspx                87389
B-2. msdn.microsoft.com/library/aa578028.aspx               207089
B-3. msdn.microsoft.com/library/jj155761.aspx                30870
B-4. msdn.microsoft.com/library/hh290140.aspx               119027
B-5. msdn.microsoft.com/library/hh524395.aspx                71260
B-6. msdn.microsoft.com/library/ms404677.aspx               199186

#Group A is complete.

B-7. msdn.microsoft.com                                            53266
B-8. msdn.microsoft.com/library/ff730837.aspx               148010

TOTAL bytes returned:  916097

C-1. msdn.microsoft.com/library/hh191443.aspx                87389
C-2. msdn.microsoft.com/library/aa578028.aspx               207089

#Group B is complete.

C-3. msdn.microsoft.com/library/jj155761.aspx                30870
C-4. msdn.microsoft.com/library/hh290140.aspx               119027
C-5. msdn.microsoft.com/library/hh524395.aspx                72765
C-6. msdn.microsoft.com/library/ms404677.aspx               199186
C-7. msdn.microsoft.com                                            56190
C-8. msdn.microsoft.com/library/ff730837.aspx               148010

TOTAL bytes returned:  920526

#Group C is complete.
```

群組 B 和 C 在群組 A 完成前啟動，但每個群組的輸出會單獨顯示。 先顯示群組 A 的所有輸出，接著是群組 B 的所有輸出，然後是群組 C 的所有輸出。應用程式一律依序顯示群組，且對每個群組，一律會根據 URL 在 URL 清單中的出現順序，顯示個別網站的相關資訊。

不過，您無法預測實際的下載順序。 啟動多個群組之後，它們產生的下載工作會全部啟用。 您不能假設 A-1 將在 B-1 之前下載，也不能假設 A-1 將在 A-2 之前下載。

#### <a name="global-definitions"></a>全域定義

範例程式碼包含所有方法都會看見的下列兩個全域宣告。

```vb
Class MainWindow    ' Class MainPage in Windows Store app.

    ' ***Declare the following variables where all methods can access them.
    Private pendingWork As Task = Nothing
    Private group As Char = ChrW(AscW("A") - 1)
```

`Task` 變數 `pendingWork` 會監督顯示程序，並防止任何群組中斷另一個群組的顯示作業。 字元變數 `group` 會標示不同群組的輸出，確認以預期的順序顯示結果。

#### <a name="the-click-event-handler"></a>Click 事件處理常式

每當使用者選擇 [開始] 按鈕，事件處理常式 `StartButton_Click` 就會增加群組字母。 處理常式接著會呼叫 `AccessTheWebAsync` 來執行下載作業。

```vb
Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)
    ' ***Verify that each group's results are displayed together, and that
    ' the groups display in order, by marking each group with a letter.
    group = ChrW(AscW(group) + 1)
    ResultsTextBox.Text &= String.Format(vbCrLf & vbCrLf & "#Starting group {0}.", group)

    Try
        ' *** Pass the group value to AccessTheWebAsync.
        Dim finishedGroup As Char = Await AccessTheWebAsync(group)

        ' The following line verifies a successful return from the download and
        ' display procedures.
        ResultsTextBox.Text &= String.Format(vbCrLf & vbCrLf & "#Group {0} is complete." & vbCrLf, finishedGroup)

    Catch ex As Exception
        ResultsTextBox.Text &= vbCrLf & "Downloads failed."

    End Try
End Sub
```

#### <a name="the-accessthewebasync-method"></a>AccessTheWebAsync 方法

此範例會將 `AccessTheWebAsync` 分為兩個方法。 第一種方法為 `AccessTheWebAsync`，可啟動群組的所有下載工作，並且設定 `pendingWork` 來控制顯示程序。 此方法會使用 Language Integrated Query (LINQ 查詢) 和 <xref:System.Linq.Enumerable.ToArray%2A> 來同時啟動所有下載工作。

`AccessTheWebAsync` 接著呼叫 `FinishOneGroupAsync` 來等候每個下載完成，並顯示它的長度。

`FinishOneGroupAsync` 傳回工作，該工作指派給 `AccessTheWebAsync` 中的 `pendingWork`。 在工作完成前，該值會防止另一項作業中斷該工作。

```vb
Private Async Function AccessTheWebAsync(grp As Char) As Task(Of Char)

    Dim client = New HttpClient()

    ' Make a list of the web addresses to download.
    Dim urlList As List(Of String) = SetUpURLList()

    ' ***Kick off the downloads. The application of ToArray activates all the download tasks.
    Dim getContentTasks As Task(Of Byte())() =
        urlList.Select(Function(addr) client.GetByteArrayAsync(addr)).ToArray()

    ' ***Call the method that awaits the downloads and displays the results.
    ' Assign the Task that FinishOneGroupAsync returns to the gatekeeper task, pendingWork.
    pendingWork = FinishOneGroupAsync(urlList, getContentTasks, grp)

    ResultsTextBox.Text &=
        String.Format(vbCrLf & "#Task assigned for group {0}. Download tasks are active." & vbCrLf, grp)

    ' ***This task is complete when a group has finished downloading and displaying.
    Await pendingWork

    ' You can do other work here or just return.
    Return grp
End Function
```

#### <a name="the-finishonegroupasync-method"></a>FinishOneGroupAsync 方法

這個方法不斷循環群組中的下載工作，等待每一項、顯示下載網站的長度，並將長度加總到總計。

`FinishOneGroupAsync` 中的第一個陳述式使用 `pendingWork`，確保進入方法不會干擾已在顯示程序中的作業，或已在等候的作業。 如果這類作業正在進行，進入作業必須等候。

```vb
Private Async Function FinishOneGroupAsync(urls As List(Of String), contentTasks As Task(Of Byte())(), grp As Char) As Task

    ' Wait for the previous group to finish displaying results.
    If pendingWork IsNot Nothing Then
        Await pendingWork
    End If

    Dim total = 0

    ' contentTasks is the array of Tasks that was created in AccessTheWebAsync.
    For i As Integer = 0 To contentTasks.Length - 1
        ' Await the download of a particular URL, and then display the URL and
        ' its length.
        Dim content As Byte() = Await contentTasks(i)
        DisplayResults(urls(i), content, i, grp)
        total += content.Length
    Next

    ' Display the total count for all of the websites.
    ResultsTextBox.Text &=
        String.Format(vbCrLf & vbCrLf & "TOTAL bytes returned:  " & total & vbCrLf)
End Function
```

您可以執行這個範例，方法是將變更貼至[建置應用程式](#BKMK_BuildingTheApp)中的程式碼，或者遵循[下載應用程式](#BKMK_DownloadingTheApp)的指示來下載範例，然後執行 QueueResults 專案。

#### <a name="points-of-interest"></a>參考資訊

在輸出中以井字號 (#) 開頭的資訊行會釐清此範例的運作方式。

輸出會顯示下列模式。

- 當前一個群組顯示其輸出時，就可以啟動一個群組，但不會中斷前一個群組的輸出顯示。

  ```console
  #Starting group A.
  #Task assigned for group A. Download tasks are active.

  A-1. msdn.microsoft.com/library/hh191443.aspx                87389
  A-2. msdn.microsoft.com/library/aa578028.aspx               207089
  A-3. msdn.microsoft.com/library/jj155761.aspx                30870
  A-4. msdn.microsoft.com/library/hh290140.aspx               119037
  A-5. msdn.microsoft.com/library/hh524395.aspx                71260

  #Starting group B.
  #Task assigned for group B. Download tasks are active.

  A-6. msdn.microsoft.com/library/ms404677.aspx               199186
  A-7. msdn.microsoft.com                                            53078
  A-8. msdn.microsoft.com/library/ff730837.aspx               148010

  TOTAL bytes returned:  915919

  B-1. msdn.microsoft.com/library/hh191443.aspx                87388
  B-2. msdn.microsoft.com/library/aa578028.aspx               207089
  B-3. msdn.microsoft.com/library/jj155761.aspx                30870

  #Group A is complete.

  B-4. msdn.microsoft.com/library/hh290140.aspx               119027
  B-5. msdn.microsoft.com/library/hh524395.aspx                71260
  B-6. msdn.microsoft.com/library/ms404677.aspx               199186
  B-7. msdn.microsoft.com                                            53078
  B-8. msdn.microsoft.com/library/ff730837.aspx               148010

  TOTAL bytes returned:  915908
  ```

- 只有第一次啟動的群組 A 才會在 `FinishOneGroupAsync` 的開頭 `pendingWork` 的 @no__t 工作。 當群組 A 到達 `FinishOneGroupAsync` 時，它尚未完成 await 運算式。 因此，尚未將控制項返回 `AccessTheWebAsync`，且尚未針對 `pendingWork` 進行第一次指派。

- 下列兩行一律會在輸出中一起出現。 在 `StartButton_Click` 中啟動群組的作業，與將群組的工作指派給 `pendingWork` 之間，程式碼永遠不會中斷

  ```console
  #Starting group B.
  #Task assigned for group B. Download tasks are active.
  ```

  在群組進入 `StartButton_Click` 後，作業尚未完成 await 運算式，需直到作業進入 `FinishOneGroupAsync` 為止。 因此，沒有其他作業可在該程式碼區段的過程中取得控制項。

## <a name="BKMD_SettingUpTheExample"></a> 檢閱及執行範例應用程式

若要進一步了解範例應用程式，您可以下載它、自行建置它，或檢閱本主題結尾的程式碼而不必實作應用程式。

> [!NOTE]
> 若要執行範例作為 Windows Presentation Foundation (WPF) 傳統型應用程式，您必須在電腦上安裝 Visual Studio 2012 或更新版本以及 .NET Framework 4.5 或更新版本。

### <a name="BKMK_DownloadingTheApp"></a> 下載應用程式

1. 從 [Async Samples:Reentrancy in .NET Desktop Apps](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06) (非同步範例︰重新進入 .NET 傳統型應用程式) 下載壓縮檔案。

2. 解壓縮您下載的檔案，然後啟動 Visual Studio。

3. 在功能表列上，依序選擇 [檔案]、[開啟舊檔]及 [專案/方案]。

4. 導覽至保存解壓縮之範例程式碼的資料夾，然後開啟方案 (.sln) 檔案。

5. 在方案總管中，開啟要執行之專案的捷徑功能表，然後選擇 [設定為啟始專案]。

6. 選擇 CTRL + F5 鍵以建置並執行專案。

### <a name="BKMK_BuildingTheApp"></a> 建置應用程式

下節提供將範例建置為 WPF 應用程式的程式碼。

##### <a name="to-build-a-wpf-app"></a>若要建置 WPF 應用程式

1. 啟動 Visual Studio。

2. 在功能表列上，選擇 [檔案]、[新增]、[專案]。

     [ **新增專案** ] 對話方塊隨即開啟。

3. 在 [**已安裝的範本**] 窗格中，展開 [ **Visual Basic**]，然後展開 [ **Windows**]。

4. 在專案類型清單中，選擇 [WPF 應用程式]。

5. 將專案命名為 `WebsiteDownloadWPF`，然後選擇 [確定] 按鈕。

     新的專案隨即會出現在方案總管中。

6. 在 Visual Studio 程式碼編輯器中，選擇 [ **MainWindow.xaml** ] 索引標籤。

     如未顯示索引標籤，請在方案總管中開啟 MainWindow.xaml 的捷徑功能表，然後選擇 [檢視程式碼]。

7. 在 MainWindow.xaml 的 [XAML] 檢視中，以下列程式碼取代程式碼。

    ```xaml
    <Window x:Class="MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:WebsiteDownloadWPF"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid Width="517" Height="360">
            <Button x:Name="StartButton" Content="Start" HorizontalAlignment="Left" Margin="-1,0,0,0" VerticalAlignment="Top" Click="StartButton_Click" Height="53" Background="#FFA89B9B" FontSize="36" Width="518"  />
            <TextBox x:Name="ResultsTextBox" HorizontalAlignment="Left" Margin="-1,53,0,-36" TextWrapping="Wrap" VerticalAlignment="Top" Height="343" FontSize="10" ScrollViewer.VerticalScrollBarVisibility="Visible" Width="518" FontFamily="Lucida Console" />
        </Grid>
    </Window>
    ```

     包含文字方塊和按鈕的簡易視窗會出現在 MainWindow.xaml 的 [設計] 檢視中。

8. 加入 <xref:System.Net.Http> 的參考。

9. 在**方案總管**中，開啟 mainwindow.xaml 的快捷方式功能表，然後選擇 [ **View Code**]。

10. 在 Mainwindow.xaml 中，將程式碼取代為下列程式碼。

    ```vb
    ' Add the following Imports statements, and add a reference for System.Net.Http.
    Imports System.Net.Http
    Imports System.Threading

    Class MainWindow

        Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)
            ' This line is commented out to make the results clearer in the output.
            'ResultsTextBox.Text = ""

            Try
                Await AccessTheWebAsync()

            Catch ex As Exception
                ResultsTextBox.Text &= vbCrLf & "Downloads failed."

            End Try
        End Sub

        Private Async Function AccessTheWebAsync() As Task

            ' Declare an HttpClient object.
            Dim client = New HttpClient()

            ' Make a list of web addresses.
            Dim urlList As List(Of String) = SetUpURLList()

            Dim total = 0
            Dim position = 0

            For Each url In urlList
                ' GetByteArrayAsync returns a task. At completion, the task
                ' produces a byte array.
                Dim urlContents As Byte() = Await client.GetByteArrayAsync(url)

                position += 1
                DisplayResults(url, urlContents, position)

                ' Update the total.
                total += urlContents.Length
            Next

            ' Display the total count for all of the websites.
            ResultsTextBox.Text &=
                String.Format(vbCrLf & vbCrLf & "TOTAL bytes returned:  " & total & vbCrLf)
        End Function

        Private Function SetUpURLList() As List(Of String)
            Dim urls = New List(Of String) From
            {
                "https://msdn.microsoft.com/library/hh191443.aspx",
                "https://msdn.microsoft.com/library/aa578028.aspx",
                "https://msdn.microsoft.com/library/jj155761.aspx",
                "https://msdn.microsoft.com/library/hh290140.aspx",
                "https://msdn.microsoft.com/library/hh524395.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            }
            Return urls
        End Function

        Private Sub DisplayResults(url As String, content As Byte(), pos As Integer)
            ' Display the length of each website. The string format is designed
            ' to be used with a monospaced font, such as Lucida Console or
            ' Global Monospace.

            ' Strip off the "http:'".
            Dim displayURL = url.Replace("https://", "")
            ' Display position in the URL list, the URL, and the number of bytes.
            ResultsTextBox.Text &= String.Format(vbCrLf & "{0}. {1,-58} {2,8}", pos, displayURL, content.Length)
        End Sub
    End Class
    ```

11. 選擇 CTRL+F5 鍵以執行程式，然後選擇 [開始] 按鈕數次。

12. 從[停用開始按鈕](#BKMK_DisableTheStartButton)、[取消後再重新啟動作業](#BKMK_CancelAndRestart)或[執行多個作業並將輸出加入佇列](#BKMK_RunMultipleOperations)進行變更以處理重新進入。

## <a name="see-also"></a>另請參閱

- [逐步解說：使用 Async 和 Await 存取 Web （Visual Basic） ](../../../../visual-basic/programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md)
- [使用 Async 和 Await 進行非同步程式設計 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/async/index.md)
