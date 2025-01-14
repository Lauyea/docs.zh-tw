---
title: C# 運算式
ms.date: 03/30/2017
ms.assetid: 29110be7-f4e3-407e-8dbe-78102eb21115
ms.openlocfilehash: c8417217064fcc1f7de5b1a9b8055743fc8cd263
ms.sourcegitcommit: d6e27023aeaffc4b5a3cb4b88685018d6284ada4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2019
ms.locfileid: "67660648"
---
# <a name="c-expressions"></a>C# 運算式
從.NET Framework 4.5 中，C#中 Windows Workflow Foundation (WF) 支援運算式。 新C#的目標.NET Framework 4.5 使用工作流程專案在 Visual Studio 2012 中建立C#運算式和 Visual Basic 工作流程專案中使用 Visual Basic 運算式。 不論是否支援專案語言，使用 Visual Basic 運算式的現有 [!INCLUDE[netfx40_short](../../../includes/netfx40-short-md.md)] 工作流程專案均可移轉至 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)]。 本主題提供 [!INCLUDE[wf1](../../../includes/wf1-md.md)] 中的 C# 運算式概觀。

## <a name="using-c-expressions-in-workflows"></a>在工作流程中使用 C# 運算式

- [在工作流程設計工具中使用 C# 運算式](csharp-expressions.md#WFDesigner)

  - [回溯相容性](csharp-expressions.md#BackwardCompat)

- [在程式碼工作流程中使用 C# 運算式](csharp-expressions.md#CodeWorkflows)

- [XAML 工作流程中使用 C# 運算式](csharp-expressions.md#XamlWorkflows)

  - [已編譯的 Xaml](csharp-expressions.md#CompiledXaml)

  - [鬆散的 Xaml](csharp-expressions.md#LooseXaml)

- [在 XAMLX 工作流程服務中使用 C# 運算式](csharp-expressions.md#WFServices)

### <a name="WFDesigner"></a> 在工作流程設計工具中使用 C# 運算式

從.NET Framework 4.5 中，C#中 Windows Workflow Foundation (WF) 支援運算式。 C#在 Visual Studio 2012 中建立使用.NET Framework 4.5 為目標的工作流程專案C#運算式，而 Visual Basic 工作流程專案則使用 Visual Basic 運算式。 若要指定所需的 C# 運算式，方塊中輸入標示**輸入 C# 運算式**。 在設計工具中選取活動時，會在屬性視窗中顯示此標籤，而此標籤也會顯示在工作流程設計工具中的活動之上。 在下列範例中，`WriteLine` 內的 `Sequence` 包含兩個 `NoPersistScope` 活動。

![螢幕擷取畫面會顯示自動建立的序列活動。](./media/csharp-expressions/auto-surround-sequence-activity.png)

> [!NOTE]
> C# 運算式只支援 Visual Studio 中，並在重新裝載工作流程設計工具中不支援。 如需有關支援在重新裝載設計工具中的新 WF45 功能的詳細資訊，請參閱[的新 Workflow Foundation 4.5 功能，在重新裝載工作流程設計工具支援](wf-features-in-the-rehosted-workflow-designer.md)。

#### <a name="BackwardCompat"></a> 回溯相容性

支援已移轉至 [!INCLUDE[netfx40_short](../../../includes/netfx40-short-md.md)] 之現有 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] C# 工作流程專案中的 Visual Basic 運算式。 當在工作流程設計工具中檢視 Visual Basic 運算式時，取代現有的 Visual Basic 運算式的文字**XAML 中設定的值**，除非 Visual Basic 運算式是有效的 C# 語法。 如果 Visual Basic 運算式為有效的 C# 語法，則會顯示該運算式。 若要將 Visual Basic 運算式更新為 C#，您可以在工作流程設計工具中編輯這些運算式，並指定相等的 C# 運算式。 您不需要將 Visual Basic 運算式更新為 C#，不過一旦這些運算式在工作流程設計工具中更新，將會轉換為 C#，且可能無法還原為 Visual Basic。

### <a name="CodeWorkflows"></a> 在程式碼工作流程中使用 C# 運算式

[!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 程式碼為主的工作流程支援 C# 運算式，但是您必須使用 <xref:System.Activities.XamlIntegration.TextExpressionCompiler.Compile%2A?displayProperty=nameWithType> 編譯 C# 運算式，工作流程才能叫用運算式。 工作流程作者可以使用 `CSharpValue` 來表示運算式的右值 (r-value)，並使用 `CSharpReference` 來表示運算式的左值 (l-value)。 下列範例使用 `Assign` 活動內含的 `WriteLine` 活動和 `Sequence` 活動，來建立工作流程。 該範例為 `CSharpReference` 的 `To` 引數，指定一個 `Assign`，表示運算式的左值。 `CSharpValue` 的 `Value` 引數和 `Assign` 的 `Text` 引數指定一個 `WriteLine`，表示這兩個運算式的右值。

```csharp
Variable<int> n = new Variable<int>
{
    Name = "n"
};

Activity wf = new Sequence
{
    Variables = { n },
    Activities =
    {
        new Assign<int>
        {
            To = new CSharpReference<int>("n"),
            Value = new CSharpValue<int>("new Random().Next(1, 101)")
        },
        new WriteLine
        {
            Text = new CSharpValue<string>("\"The number is \" + n")
        }
    }
};

CompileExpressions(wf);

WorkflowInvoker.Invoke(wf);
```

建構工作流程之後，會呼叫 `CompileExpressions` Helper 方法來編譯 C# 運算式，然後叫用工作流程。 以下範例是 `CompileExpressions` 方法。

```csharp
static void CompileExpressions(Activity activity)
{
    // activityName is the Namespace.Type of the activity that contains the
    // C# expressions.
    string activityName = activity.GetType().ToString();

    // Split activityName into Namespace and Type.Append _CompiledExpressionRoot to the type name
    // to represent the new type that represents the compiled expressions.
    // Take everything after the last . for the type name.
    string activityType = activityName.Split('.').Last() + "_CompiledExpressionRoot";
    // Take everything before the last . for the namespace.
    string activityNamespace = string.Join(".", activityName.Split('.').Reverse().Skip(1).Reverse());

    // Create a TextExpressionCompilerSettings.
    TextExpressionCompilerSettings settings = new TextExpressionCompilerSettings
    {
        Activity = activity,
        Language = "C#",
        ActivityName = activityType,
        ActivityNamespace = activityNamespace,
        RootNamespace = null,
        GenerateAsPartialClass = false,
        AlwaysGenerateSource = true,
        ForImplementation = false
    };

    // Compile the C# expression.
    TextExpressionCompilerResults results =
        new TextExpressionCompiler(settings).Compile();

    // Any compilation errors are contained in the CompilerMessages.
    if (results.HasErrors)
    {
        throw new Exception("Compilation failed.");
    }

    // Create an instance of the new compiled expression type.
    ICompiledExpressionRoot compiledExpressionRoot =
        Activator.CreateInstance(results.ResultType,
            new object[] { activity }) as ICompiledExpressionRoot;

    // Attach it to the activity.
    CompiledExpressionInvoker.SetCompiledExpressionRoot(
        activity, compiledExpressionRoot);
}
```

> [!NOTE]
> 如果C#運算式不會經過編譯，<xref:System.NotSupportedException>時，使用類似下列的訊息叫用工作流程時擲回：`Expression Activity type 'CSharpValue`1' 需要編譯，才能執行。  請確定已編譯工作流程。 '

如果您的自訂程式碼為主之工作流程使用 `DynamicActivity`，則需要對 `CompileExpressions` 方法進行一些變更，如下列程式碼範例所示。

```csharp
static void CompileExpressions(DynamicActivity dynamicActivity)
{
    // activityName is the Namespace.Type of the activity that contains the
    // C# expressions. For Dynamic Activities this can be retrieved using the
    // name property , which must be in the form Namespace.Type.
    string activityName = dynamicActivity.Name;

    // Split activityName into Namespace and Type.Append _CompiledExpressionRoot to the type name
    // to represent the new type that represents the compiled expressions.
    // Take everything after the last . for the type name.
    string activityType = activityName.Split('.').Last() + "_CompiledExpressionRoot";
    // Take everything before the last . for the namespace.
    string activityNamespace = string.Join(".", activityName.Split('.').Reverse().Skip(1).Reverse());

    // Create a TextExpressionCompilerSettings.
    TextExpressionCompilerSettings settings = new TextExpressionCompilerSettings
    {
        Activity = dynamicActivity,
        Language = "C#",
        ActivityName = activityType,
        ActivityNamespace = activityNamespace,
        RootNamespace = null,
        GenerateAsPartialClass = false,
        AlwaysGenerateSource = true,
        ForImplementation = true
    };

    // Compile the C# expression.
    TextExpressionCompilerResults results =
        new TextExpressionCompiler(settings).Compile();

    // Any compilation errors are contained in the CompilerMessages.
    if (results.HasErrors)
    {
        throw new Exception("Compilation failed.");
    }

    // Create an instance of the new compiled expression type.
    ICompiledExpressionRoot compiledExpressionRoot =
        Activator.CreateInstance(results.ResultType,
            new object[] { dynamicActivity }) as ICompiledExpressionRoot;

    // Attach it to the activity.
    CompiledExpressionInvoker.SetCompiledExpressionRootForImplementation(
        dynamicActivity, compiledExpressionRoot);
}
```

在動態活動中編譯 C# 運算式的 `CompileExpressions` 多載有一些差異。

- 傳給 `CompileExpressions` 的參數為 `DynamicActivity`。

- 擷取型別名稱和命名空間需使用 `DynamicActivity.Name` 屬性。

- `TextExpressionCompilerSettings.ForImplementation` 設定為 `true`。

- 呼叫 `CompiledExpressionInvoker.SetCompiledExpressionRootForImplementation`，而不是 `CompiledExpressionInvoker.SetCompiledExpressionRoot`。

如需有關使用程式碼中的運算式的詳細資訊，請參閱[撰寫工作流程、 活動和運算式使用命令式程式碼](authoring-workflows-activities-and-expressions-using-imperative-code.md)。

### <a name="XamlWorkflows"></a> XAML 工作流程中使用 C# 運算式

XAML 工作流程支援 C# 運算式。 編譯的 XAML 工作流程會編譯為型別，而鬆散的 XAML 工作流程會在工作流程執行時，由執行階段載入並編譯為活動樹狀。

- [已編譯的 Xaml](csharp-expressions.md#CompiledXaml)

- [鬆散的 Xaml](csharp-expressions.md#LooseXaml)

#### <a name="CompiledXaml"></a> 已編譯的 Xaml

編譯為型別的 XAML 工作流程支援 C# 運算式，當做部分以 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 為目標的 C# 工作流程專案。 已編譯的 XAML 是在 Visual Studio 中撰寫工作流程的預設型別和 C# 工作流程專案中建立 Visual Studio 目標[!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)]使用 C# 運算式。

#### <a name="LooseXaml"></a> 鬆散的 Xaml

鬆散的 XAML 工作流程支援 C# 運算式。 載入及叫用鬆散的 XAML 工作流程所用之工作流程主機程式，必須以 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 為目標，且 <xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings.CompileExpressions%2A> 必須設定為 `true` (預設值為 `false`)。 若要將 <xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings.CompileExpressions%2A> 設定為 `true`，請建立 <xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings> 執行個體並將其 <xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings.CompileExpressions%2A> 屬性設定為 `true`，然後當做參數傳給 <xref:System.Activities.XamlIntegration.ActivityXamlServices.Load%2A?displayProperty=nameWithType>。 如果`CompileExpressions`未設為`true`、<xref:System.NotSupportedException>就會擲回類似下列訊息：`Expression Activity type 'CSharpValue`1' 需要編譯，才能執行。  請確定已編譯工作流程。 '

```csharp
ActivityXamlServicesSettings settings = new ActivityXamlServicesSettings
{
    CompileExpressions = true
};

DynamicActivity<int> wf = ActivityXamlServices.Load(new StringReader(serializedAB), settings) as DynamicActivity<int>;
```

如需有關使用 XAML 工作流程的詳細資訊，請參閱[序列化工作流程和活動與 XAML](serializing-workflows-and-activities-to-and-from-xaml.md)。

### <a name="WFServices"></a> 在 XAMLX 工作流程服務中使用 C# 運算式

XAMLX 工作流程服務支援 C# 運算式。 如果工作流程服務是以 IIS 或 WAS 裝載，則不需要其他步驟；但是，如果 XAML 工作流程服務為自我裝載，則必須編譯 C# 運算式。 若要編譯的自我裝載的 XAMLX 工作流程服務中的 C# 運算式，先將 XAMLX 檔案載入到`WorkflowService`，然後將傳遞`Body`的`WorkflowService`來`CompileExpressions`先前所述的方法[使用 C#在程式碼工作流程中的使用運算式](csharp-expressions.md#CodeWorkflows)一節。 下列範例將載入 XAMLX 工作流程服務、編譯 C# 運算式，然後開啟工作流程服務並等候要求。

```csharp
// Load the XAMLX workflow service.
WorkflowService workflow1 =
    (WorkflowService)XamlServices.Load(xamlxPath);

// Compile the C# expressions in the workflow by passing the Body to CompileExpressions.
CompileExpressions(workflow1.Body);

// Initialize the WorkflowServiceHost.
var host = new WorkflowServiceHost(workflow1, new Uri("http://localhost:8293/Service1.xamlx"));

// Enable Metadata publishing/
ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
smb.HttpGetEnabled = true;
smb.MetadataExporter.PolicyVersion = PolicyVersion.Policy15;
host.Description.Behaviors.Add(smb);

// Open the WorkflowServiceHost and wait for requests.
host.Open();
Console.WriteLine("Press enter to quit");
Console.ReadLine();
```

如果未編譯 C# 運算式，`Open` 作業會成功，但工作流程在叫用時會失敗。 下列`CompileExpressions`方法是從先前的方法相同[程式碼工作流程中的使用 C# 運算式](csharp-expressions.md#CodeWorkflows)一節。

```csharp
static void CompileExpressions(Activity activity)
{
    // activityName is the Namespace.Type of the activity that contains the
    // C# expressions.
    string activityName = activity.GetType().ToString();

    // Split activityName into Namespace and Type.Append _CompiledExpressionRoot to the type name
    // to represent the new type that represents the compiled expressions.
    // Take everything after the last . for the type name.
    string activityType = activityName.Split('.').Last() + "_CompiledExpressionRoot";
    // Take everything before the last . for the namespace.
    string activityNamespace = string.Join(".", activityName.Split('.').Reverse().Skip(1).Reverse());

    // Create a TextExpressionCompilerSettings.
    TextExpressionCompilerSettings settings = new TextExpressionCompilerSettings
    {
        Activity = activity,
        Language = "C#",
        ActivityName = activityType,
        ActivityNamespace = activityNamespace,
        RootNamespace = null,
        GenerateAsPartialClass = false,
        AlwaysGenerateSource = true,
        ForImplementation = false
    };

    // Compile the C# expression.
    TextExpressionCompilerResults results =
        new TextExpressionCompiler(settings).Compile();

    // Any compilation errors are contained in the CompilerMessages.
    if (results.HasErrors)
    {
        throw new Exception("Compilation failed.");
    }

    // Create an instance of the new compiled expression type.
    ICompiledExpressionRoot compiledExpressionRoot =
        Activator.CreateInstance(results.ResultType,
            new object[] { activity }) as ICompiledExpressionRoot;

    // Attach it to the activity.
    CompiledExpressionInvoker.SetCompiledExpressionRoot(
        activity, compiledExpressionRoot);
}
```
