---
title: WPF 安全性策略 – 安全性工程
ms.date: 03/30/2017
helpviewer_keywords:
- security [WPF], testing techniques
- Security Development Lifecycle (SDL), security analysis [WPF]
- Security Development Lifecycle (SDL), threat modeling
- Security Development Lifecycle (SDL), testing techniques
- Security Development Lifecycle (SDL), source analysis tools
- Security Development Lifecycle (SDL), critical code management
- threat modeling [WPF]
ms.assetid: 0fc04394-4e47-49ca-b0cf-8cd1161d95b9
ms.openlocfilehash: a042f0ae1c7673f7d21b39580db3d373835939cd
ms.sourcegitcommit: da2dd2772fcf32b44eb18b1cbe8affd17b1753c9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71353827"
---
# <a name="wpf-security-strategy---security-engineering"></a>WPF 安全性策略 – 安全性工程
高可信度電腦運算是一項 Microsoft 開發案，用於確保生產安全的程式碼。 「可信賴的運算」計畫的關鍵要素是 Microsoft 安全性開發週期 (SDL) （SDL）。 SDL 是一種工程實務，與標準工程程式搭配使用，以促進安全程式碼的傳遞。 SDL 包含十個階段，結合了最佳作法與正規化、measurability 和其他結構，包括：  
  
- 安全性設計分析  
  
- 以工具為基礎的品質檢查  
  
- 滲透測試  
  
- 最終安全性檢閱  
  
- 產品發行後安全性管理  
  
## <a name="wpf-specifics"></a>WPF 特性  
 @No__t 0 工程小組會同時套用和擴充 SDL，其結合包括下列重要層面：  
  
 [威脅模型](#threat_modeling)  
  
 [安全性分析和編輯工具](#tools)  
  
 [測試技術](#techniques)  
  
 [重要程式碼管理](#critical_code)  
  
<a name="threat_modeling"></a>   
### <a name="threat-modeling"></a>威脅模型  
 威脅模型化是 SDL 的核心元件，用來分析系統以判斷潛在的安全性弱點。 一旦識別出漏洞，威脅模型也可確保適當的安全防護已就位。  
  
 高階的威脅模型牽涉到下列重要步驟，藉由使用雜貨店做為範例：  
  
1. **識別資產**。 雜貨店的資產可能包含員工、保險箱、收銀機和庫存。  
  
2. **列舉進入點**。 雜貨店的進入點可能包含前門和後門、窗戶、卸貨平台和空調設備。  
  
3. **使用進入點調查針對資產的攻擊**。 一次可能的攻擊或許會經由「空調」進入點，以雜貨店的「保險箱」資產為攻擊目標；空調設備可能被拆下，讓保險箱能從中被拖出來，搬到商店外。  
  
 威脅模型套用到整個 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 並包含下列各項：  
  
- [!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)] 剖析器會如何讀取檔案，將文字對應到對應的物件模型類別，並建立實際的程式碼。  
  
- 如何建立視窗控制代碼 (hWnd)、傳送訊息，以及用來呈現視窗的內容。  
  
- 資料繫結如何取得資源，並與系統互動。  
  
 在開發過程中，這些威脅模型對於識別安全性設計需求和威脅的安全防護相當重要。  
  
<a name="tools"></a>   
### <a name="source-analysis-and-editing-tools"></a>來源分析和編輯工具  
 除了 SDL 的手動安全性程式碼審查元素之外，@no__t 0 團隊也使用數種工具進行來源分析，並將相關的編輯專案用於減少安全性弱點。 使用廣泛的來源工具，包含下列項目：  
  
- **FXCop**：尋找 managed 程式碼中的常見安全性問題，範圍從繼承規則到代碼啟用安全性使用方式，到如何安全地與非受控程式碼交互操作。 請參閱 [FXCop](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.0/bb429476%28v=vs.80%29)。  
  
- **Prefix/Prefast**：尋找非受控程式碼中的安全性弱點和常見的安全性問題，例如緩衝區溢位、格式字串問題和錯誤檢查。  
  
- **禁止的 api**：搜尋原始程式碼，以識別已知會造成安全性問題的函式意外使用，例如 `strcpy`。 一旦識別出來，這些函式會取代為更安全的替代專案。  
  
<a name="techniques"></a>   
### <a name="testing-techniques"></a>測試技術  
 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 使用不同的安全性測試技術，包含：  
  
- **白箱測試**：測試人員會查看原始程式碼，然後建立惡意探勘測試。
  
- **黑箱測試**：測試人員會藉由檢查 API 和功能來嘗試找出安全性攻擊，然後嘗試攻擊產品。  
  
- **回歸其他產品的安全性問題**：在相關的情況下，會測試來自相關產品的安全性問題。 例如，已識別 Internet Explorer 大約60安全性問題的適當變種，並嘗試其對 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 的適用性。  
  
- 以**工具為基礎的滲透測試透過檔案模糊**處理：檔案模糊處理是透過各種輸入來利用檔案讀取器的輸入範圍。 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 中使用這項技術的其中一個範例是用來檢查影像解碼程式碼中的錯誤。  
  
<a name="critical_code"></a>   
### <a name="critical-code-management"></a>重要程式碼管理  
 對於 [!INCLUDE[TLA#tla_xbap#plural](../../../includes/tlasharptla-xbapsharpplural-md.md)]，[!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 會使用 .NET Framework 支援來建立安全性沙箱，以標示和追蹤提升許可權的安全性關鍵程式碼（請參閱[WPF 安全性策略-平臺安全性](wpf-security-strategy-platform-security.md)中的**安全性關鍵方法**）。 假設在安全性關鍵程式碼上的安全性需求很高，這類程式碼會接收額外層級的來源管理控制和安全性稽核。 大約 5% 到 10%的 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 由經過專門的檢閱小組檢閱的安全性關鍵程式碼所組成。 原始程式碼和簽入程序是由追蹤安全性關鍵程式碼來管理，以及對應每個重要的實體 (也就是一個包含關鍵程式碼的方法) 至其登出狀態。 登出狀態包含一或多個檢閱者的名稱。 每個 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 的每日建置會比較該關鍵程式碼和前一個建置的關鍵程式碼，以檢查未經核准的變更。 如果工程師修改未經檢閱小組核准的關鍵程式碼，它會被識別並被立即修正。 此程序能在 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 沙箱程式碼上套用和維護特別高層級的監督。  
  
## <a name="see-also"></a>另請參閱

- [安全性](security-wpf.md)
- [WPF 部分信任安全性](wpf-partial-trust-security.md)
- [WPF 安全性策略 – 平台安全性](wpf-security-strategy-platform-security.md)
- [高可信度電腦運算](https://www.microsoft.com/mscorp/twc/default.mspx)
- [.NET 中的安全性](../../standard/security/index.md)
