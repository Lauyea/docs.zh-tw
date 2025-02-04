---
title: 作法：撰寫複合控制項
ms.date: 03/30/2017
helpviewer_keywords:
- UserControl class [Windows Forms], creating composite controls
- user controls [Windows Forms], creating
- user controls [Windows Forms], inheriting from
- composite controls [Windows Forms], creating
ms.assetid: 79c9cf05-5ab6-4a18-886d-88a64748b098
author: gewarren
ms.author: gewarren
manager: jillfra
ms.openlocfilehash: 08cb07ceebf08b3096415f9b7370e2d955152cb6
ms.sourcegitcommit: 121ab70c1ebedba41d276e436dd2b1502748a49f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/24/2019
ms.locfileid: "70015927"
---
# <a name="how-to-author-composite-controls"></a>作法：撰寫複合控制項

複合控制項的運用方式有許多種。 您可以將它們撰寫成 Windows 桌面應用程式專案的一部分，且只在專案中的表單上使用它們。 或者，您可以在 Windows 控制項程式庫專案中撰寫它們、將專案編譯成組件，然後在其他專案中使用控制項。 您甚至可以從它們繼承, 並使用視覺繼承來快速地自訂它們以進行特殊用途。

## <a name="to-author-a-composite-control"></a>撰寫複合控制項

1. 在 Visual Studio 中, 建立新的**Windows 應用程式**專案, 並將其命名為**DemoControlHost**。

2. 在 [專案] 功能表上，按一下 [加入使用者控制項]。

3. 在 [新增項目] 對話方塊中，為類別檔案 (.vb 或 .cs 檔案) 指定您希望複合控制項擁有的名稱。

4. 選取 [**新增**] 按鈕, 以建立複合控制項的類別檔案。

5. 將控制項從 [工具箱] 新增至複合控制項介面。

6. 將程式碼放在事件程序中，以處理複合控制項或其組成控制項所引發的事件。

7. 關閉複合控制項的設計工具，並在系統提示時儲存檔案。

8. 在 [ **建置** ] 功能表上，按一下 [ **建置方案**]。

     必須建置專案，以便自訂要顯示於 [工具箱] 的控制項。

9. 使用 [工具箱] 的 [DemoControlHost] 索引標籤，將控制項的執行個體新增至 `Form1`。

## <a name="to-author-a-control-class-library"></a>撰寫控制項類別庫

1. 開啟新的 [Windows 控制項程式庫] 專案。

     根據預設，專案會包含複合控制項。

2. 如上述程序所述，新增控制項和程式碼。

3. 選擇您不希望繼承類別變更的控制項，並將此控制項的 [修飾詞] 屬性設定為 [私人]。

4. 建置 DLL。

## <a name="to-inherit-from-a-composite-control-in-a-control-class-library"></a>繼承自控制項類別庫中的複合控制項

1. 在 [檔案] 功能表上，指向 [新增]，然後選取 [新增專案]，以將新的 [Windows 應用程式] 專案新增至方案。

2. 在 [方案總管] 中，以滑鼠右鍵按一下新專案的 [參考] 資料夾，然後選擇 [新增參考] 以開啟 [新增參考] 對話方塊。

3. 選取 [專案] 索引標籤並連按兩下您的控制項程式庫專案。

4. 在 [ **建置** ] 功能表上，按一下 [ **建置方案**]。

5. 在 [方案總管] 中，以滑鼠右鍵按一下您的控制項程式庫專案並選取捷徑功能表中的 [新增項目]。

6. 從 [新增項目] 對話方塊中，選取 [繼承的使用者控制項] 範本。

7. 在 [繼承選取器] 對話方塊方塊中，連按兩下您想要繼承的控制項。

     新的控制項會新增至您的專案。

8. 開啟新控制項的視覺化設計工具並新增其他組成控制項。

     您可以在 DLL 中看見繼承自複合控制項的組成控制項，而且可以修改其 [修飾詞] 屬性為 [公用] 之控制項的屬性。 您無法變更其 [修飾詞] 屬性為 [私人] 之控制項的屬性。

## <a name="see-also"></a>另請參閱

- [逐步解說：撰寫複合控制項](walkthrough-authoring-a-composite-control-with-visual-csharp.md)
- [逐步解說：繼承自 Windows Forms 控制項](walkthrough-inheriting-from-a-windows-forms-control-with-visual-csharp.md)
- [控制項類型建議](control-type-recommendations.md)
- [如何：Windows Forms 的作者控制項](how-to-author-controls-for-windows-forms.md)
- [各種自訂控制項](varieties-of-custom-controls.md)
