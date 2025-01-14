---
title: 利用 UI 自動化取得混合文字屬性的詳細資料
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d0e4c005-abd1-42bb-92a4-5faf87097311
ms.openlocfilehash: 13ebc6aefe925ecefe48a9b0fa8cf7a6ecd3c454
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2019
ms.locfileid: "71042944"
---
# <a name="obtain-mixed-text-attribute-details-using-ui-automation"></a>利用 UI 自動化取得混合文字屬性的詳細資料
> [!NOTE]
> 這份文件適用於想要使用 <xref:System.Windows.Automation> 命名空間中定義之 Managed [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 類別的 .NET Framework 開發人員。 如需的最新[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]資訊, [請參閱 Windows Automation API:使用者介面](https://go.microsoft.com/fwlink/?LinkID=156746)自動化。  
  
 本主題說明如何使用 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] ，從跨越多個屬性值的文字範圍中取得文字屬性詳細資料。 文字範圍可以對應至文件內目前的插入號位置 (或變質選取範圍)、連續文字選取範圍、非連續文字選取範圍的集合，或文件的整個文字內容。  
  
## <a name="example"></a>範例  
 下列程式碼範例說明如何取得文字範圍中的 <xref:System.Windows.Automation.TextPattern.FontNameAttribute> ，其中 <xref:System.Windows.Automation.Text.TextPatternRange.GetAttributeValue%2A> 會傳回 <xref:System.Windows.Automation.TextPattern.MixedAttributeValue> 物件。  
  
[!code-csharp[FindText#RetrieveMixedAttributes](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#retrievemixedattributes)]
[!code-vb[FindText#RetrieveMixedAttributes](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#retrievemixedattributes)]  
  
 <xref:System.Windows.Automation.TextPattern> 控制項模式 (結合 <xref:System.Windows.Automation.Text.TextPatternRange> 類別) 可支援基本文字屬性 (Attribute)、屬性 (Property) 及方法。 如果是 <xref:System.Windows.Automation.TextPattern> 或 <xref:System.Windows.Automation.Text.TextPatternRange>不支援的控制項特定功能， <xref:System.Windows.Automation.AutomationElement> 類別會提供使用者介面自動化用戶端方法，以存取對應的原生物件模型。  
  
## <a name="see-also"></a>另請參閱

- [UI 自動化 TextPattern 概觀](ui-automation-textpattern-overview.md)
- [使用 UI 自動化將內容新增至文字方塊](add-content-to-a-text-box-using-ui-automation.md)
- [使用 UI 自動化尋找和反白顯示文字](find-and-highlight-text-using-ui-automation.md)
- [UI 自動化控制項模式概觀](ui-automation-control-patterns-overview.md)
- [用戶端的 UI 自動化控制項模式](ui-automation-control-patterns-for-clients.md)
- [使用 UI 自動化取得文字屬性](obtain-text-attributes-using-ui-automation.md)
