---
title: 實作 UI 自動化 MultipleView 控制項模式
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, MultipleView control pattern
- MultipleView control pattern
- control patterns, MultipleView
ms.assetid: 5bf1b248-ffee-48c8-9613-0b134bbe9f6a
ms.openlocfilehash: 699644b98fbf818c71553775f4dff8dfb0726977
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2019
ms.locfileid: "71043434"
---
# <a name="implementing-the-ui-automation-multipleview-control-pattern"></a>實作 UI 自動化 MultipleView 控制項模式
> [!NOTE]
> 這份文件適用於想要使用 <xref:System.Windows.Automation> 命名空間中定義之 Managed [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 類別的 .NET Framework 開發人員。 如需的最新[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]資訊, [請參閱 Windows Automation API:使用者介面](https://go.microsoft.com/fwlink/?LinkID=156746)自動化。  
  
 本主題將介紹實作 <xref:System.Windows.Automation.Provider.IMultipleViewProvider>的方針和慣例，包括事件和屬性的相關資訊。 其他參考的連結列於主題的結尾。  
  
 <xref:System.Windows.Automation.MultipleViewPattern> 控制項模式可用來支援控制項，這種控制項提供相同一組資訊或子控制項的多種不同表示，而且能夠在這些表示之間切換。  
  
 可以顯示多個視圖的控制項範例包括 [清單] 視圖（可以將其內容顯示為縮圖、磚、圖示或詳細資料[!INCLUDE[TLA#tla_xl](../../../includes/tlasharptla-xl-md.md)] ）、圖表（圓形圖、折線圖、橫條圖、具有公式的單元[!INCLUDE[TLA#tla_word](../../../includes/tlasharptla-word-md.md)]格值）、檔（一般、Web 版面配置、列印版面配置、閱讀版面配置、大綱）、Microsoft Outlook 行事曆（年、月、周、日[!INCLUDE[TLA#tla_wmp](../../../includes/tlasharptla-wmp-md.md)] ）和外觀。 支援哪些檢視會由控制項的開發人員決定，而且是每個控制項所特有。  
  
<a name="Implementation_Guidelines_and_Conventions"></a>   
## <a name="implementation-guidelines-and-conventions"></a>實作方針和慣例  
 實作多重檢視控制項模式時，請注意下列方針和慣例：  
  
- 若管理目前檢視的容器與提供目前檢視的控制項不同，則也應在容器上實作<xref:System.Windows.Automation.Provider.IMultipleViewProvider> 。 例如，Windows 檔案總管包含目前資料夾內容的清單控制項，而控制項的檢視則是由 Windows 檔案總管應用程式管理。  
  
- 可以排序內容的控制項不視為支援多種檢視。  
  
- 檢視集合在執行個體之間必須完全相同。  
  
- 檢視名稱必須適用於文字轉換語音、點字以及其他人類看得懂的應用程式。  
  
<a name="Required_Members_for_IMultipleViewProvider"></a>   
## <a name="required-members-for-imultipleviewprovider"></a>IMultipleViewProvider 的必要成員  
 以下是實作 IMultipleViewProvider 的必要屬性和方法。  
  
|必要成員|成員類型|注意|  
|----------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.IMultipleViewProvider.CurrentView%2A>|屬性|None|  
|<xref:System.Windows.Automation.Provider.IMultipleViewProvider.GetSupportedViews%2A>|方法|None|  
|<xref:System.Windows.Automation.Provider.IMultipleViewProvider.GetViewName%2A>|方法|無|  
|<xref:System.Windows.Automation.Provider.IMultipleViewProvider.SetCurrentView%2A>|方法|無|  
  
 這個控制項模式沒有相關事件。  
  
<a name="Exceptions"></a>   
## <a name="exceptions"></a>例外狀況  
 提供者必須擲回下列例外狀況。  
  
|例外狀況類型|條件|  
|--------------------|---------------|  
|<xref:System.ArgumentException>|呼叫 <xref:System.Windows.Automation.Provider.IMultipleViewProvider.SetCurrentView%2A> 或 <xref:System.Windows.Automation.Provider.IMultipleViewProvider.GetViewName%2A> 時使用的參數不是所支援檢視集合的成員。|  
  
## <a name="see-also"></a>另請參閱

- [UI 自動化控制項模式概觀](ui-automation-control-patterns-overview.md)
- [支援 UI 自動化提供者的控制項模式](support-control-patterns-in-a-ui-automation-provider.md)
- [用戶端的 UI 自動化控制項模式](ui-automation-control-patterns-for-clients.md)
- [UI 自動化樹狀目錄概觀](ui-automation-tree-overview.md)
- [在 UI 自動化中使用快取](use-caching-in-ui-automation.md)
