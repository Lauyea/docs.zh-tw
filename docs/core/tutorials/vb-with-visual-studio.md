---
title: 在 Visual Studio 2017 中使用 Visual Basic 來建置 .NET Core Hello World 應用程式
description: 了解如何使用 Visual Studio 2017 以 Visual Basic 建置簡單的 .NET Core 主控台應用程式。
author: rpetrusha
ms.author: ronpet
ms.date: 08/07/2017
dev_langs:
- vb
ms.custom: vs-dotnet, seodec18
ms.openlocfilehash: b4f3cc055f73332db1348ef35174beab614df147
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2019
ms.locfileid: "71039606"
---
# <a name="build-a-visual-basic-hello-world-application-with-the-net-core-sdk-in-visual-studio-2017"></a>在 Visual Studio 2017 中使用 .NET Core SDK 來建置 Visual Basic Hello World 應用程式

本主題提供使用 Visual Studio 2017 的 Visual Basic，針對簡單 .NET Core 主控台應用程式進行建置、偵錯及發行的逐步指示。 Visual Studio 2017 提供功能完整的開發環境來建置 .NET Core 應用程式。 只要應用程式沒有平台特定的相依性，應用程式就可以在 .NET Core 的任何目標平台，以及安裝 .NET Core 的任何系統上執行。

## <a name="prerequisites"></a>必要條件

已安裝「.NET Core 跨平台開發」工作負載的 [Visual Studio 2017 (英文)](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs)。 您可以使用 .NET Core 2.1 或更新版本來開發您的應用程式。

如需詳細資訊，請參閱 [Windows 上 .NET Core 的必要條件](../windows-prerequisites.md)。

## <a name="a-simple-hello-world-application"></a>簡單的 Hello World 應用程式

請開始建立簡單的 "Hello World" 主控台應用程式。 請依照下列步驟：

1. 啟動 Visual Studio 2017。 從功能表列中選取 [檔案]  >  [新增]  >  [專案]。 在 [新增專案]* 對話方塊中，選取後面跟著 [.NET Core] 節點的 [Visual Basic] 節點。 然後選取 [主控台應用程式 (.NET Core)] 專案範本。 在 [名稱] 文字方塊中，鍵入 "HelloWorld"。 選取 [確定] 按鈕。

   ![已選取 [主控台應用程式] 的 [新增專案] 對話方塊](./media/vb-with-visual-studio/visual-studio-new-project.png)

1. Visual Studio 使用範本建立專案。 .NET Core 的 Visual Basic 主控台應用程式範本會自動定義一個 `Program` 類別，該類別具有採用 <xref:System.String> 陣列為引數的單一 `Main` 方法。 `Main` 是應用程式進入點，是執行階段在啟動應用程式時會自動呼叫的方法。 在應用程式啟動時所提供的所有命令列引數，都會在 *args* 陣列中提供。

   ![Visual Studio 和新的 HelloWorld 專案](./media/vb-with-visual-studio/visual-studio-main-window.png)

   此範本會建立簡單的 "Hello World" 應用程式。 它會呼叫 <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> 方法，以在主控台視窗中顯示常值字串 "Hello World!" 。 透過使用工具列上的綠色箭頭選取 **HelloWorld** 按鈕，您可以 [偵錯] 模式執行程式。 不過，如果您這樣做，主控台視窗將會在簡短顯示之後關閉。 這是因為在 `Main` 方法中的單一陳述式執行完畢之後，`Main` 方法會立即終止，而應用程式會立即結束。

1. 若要使應用程式在關閉主控台視窗之前先暫停，請在 <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> 方法呼叫之後立即新增下列程式碼：

   ```vb
   Console.Write("Press any key to continue...")
   Console.ReadKey(true)
   ```

   此程式碼會提示使用者按下任意鍵，並暫停程式直到按下按鍵為止。

1. 在功能表列中，選取 [組建]  >  [組建方案]。 這會將您的程式編譯成中繼語言 (IL)，而該語言是由 Just-In-Time (JIT) 編譯器轉換為二進位程式碼。

1. 透過使用工具列上的綠色箭頭選取 **HelloWorld** 按鈕來執行程式。

   ![主控台視窗顯示 Hello World，請按任意鍵以繼續](./media/with-visual-studio/hello-world-console.png)

1. 按任意鍵以關閉主控台視窗。

## <a name="enhancing-the-hello-world-application"></a>增強 Hello World 應用程式

增強您的應用程式以提示使用者輸入其名稱，與日期和時間一起顯示。 若要修改並測試程式，請執行下列動作︰

1. 在程式碼視窗中，於 `Sub Main(args As String())` 行之後的左括弧後方，以及第一個右括弧之前輸入下列 Visual Basic 程式碼：

   [!code-vb[GettingStarted#1](../../../samples/snippets/core/tutorials/vb-with-visual-studio/helloworld.vb#1)]

   此程式碼會取代 `Main` 方法的內容。

   ![含有已更新之 Main 方法的 Visual Studio 程式檔案](./media/vb-with-visual-studio/visual-basic-code-window.png)

   此程式碼會在主控台中顯示「What is your name?」， 然後等待使用者輸入字串並按 Enter 鍵。 它會將此字串儲存至名為 `name` 的變數。 此程式碼也會擷取 <xref:System.DateTime.Now?displayProperty=nameWithType> 屬性的值，其中包含目前的當地時間，並將它指派至名稱為 `currentDate` 的變數。 最後，使用[字串插值](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md)在主控台視窗中顯示這些值。

1. 選擇 [建置]  >  [建置方案] 來編譯程式。

1. 在 Visual Studio 中以 [偵錯] 模式執行程式，方法為選取工具列上的綠色箭頭，按 F5，或是選擇 [偵錯]  >  [開始偵錯] 功能表項目。 輸入名稱並按 Enter 鍵來回應提示。

   ![含有已修改程式輸出的主控台視窗](./media/with-visual-studio/hello-world-update.png)

1. 按任意鍵以關閉主控台視窗。

您已建立並執行應用程式。 若要開發專業的應用程式，請執行一些其他步驟，來針對發行準備應用程式：

- 若要針對您的應用程式進行偵錯，請參閱[使用 Visual Studio 2017 針對您的 .NET Core Hello World 應用程式進行偵錯](debugging-with-visual-studio.md)。

- 若要發行應用程式可散發版本，請參閱[使用 Visual Studio 2017 發行您的 .NET Core Hello World 應用程式](publishing-with-visual-studio.md)。

## <a name="related-topics"></a>相關主題

除了主控台應用程式之外，您也可以使用 Visual Basic、.NET Core 和 Visual Studio 2017 建置 .NET Standard 類別庫。 如需逐步介紹，請參閱[在 Visual Studio 2017 中使用 Visual Basic 和 .NET Core SDK 建置 .NET Standard 程式庫](vb-library-with-visual-studio.md)。
