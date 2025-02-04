---
title: .NET Framework 中的應用程式相容性
ms.date: 05/19/2017
helpviewer_keywords:
- application compatibility
- .NET Framework application compatibility
- .NET Framework changes
ms.assetid: c4ba3ff2-fe59-4c5d-9e0b-86bba3cd865c
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: f547180995ec155f9121eeace109e7dfb07c7827
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70790120"
---
# <a name="application-compatibility-in-the-net-framework"></a>.NET Framework 中的應用程式相容性

## <a name="introduction"></a>簡介
相容性是每個 .NET 版本都極為重視的目標。 相容性可確保每個版本都能累加，讓舊版仍能運作。 另一方面，舊版功能的變更 (為改善效能、處理安全性問題，或修正 Bug) 會造成現有程式碼或現有應用程式中在更新版本下執行的相容性問題。 .NET Framework 可辨識重定目標變更及執行階段變更。 重定目標變更會影響以 .NET Framework 特定版本為目標，但在更新版本上執行的應用程式。 執行階段變更會影響在特定版本上執行的所有應用程式。

每個應用程式都有特定版本的 .NET Framework 目標，可以下列方式指定：

- 在 Visual Studio 中定義目標架構。
- 在專案檔中指定目標架構。
- 將 <xref:System.Runtime.Versioning.TargetFrameworkAttribute> 套用至原始程式碼。

在比目標版本更新的版本上執行時，.NET Framework 會使用古怪的行為，模擬較舊的目標版本。 換句話說，應用程式會在較新的 Framework 版本上執行，但表現出的行為如同在較舊的版本上執行。 .NET Framework 版本間的許多相容性問題是透過這種古怪的模型而降低。 應用程式的目標 .NET Framework 版本取決於執行程式碼所在之應用程式定義域的項目組件目標版本。 該應用程式定義域載入的所有其他組件則以該 .NET Framework 版本為目標。 例如，在可執行檔的情況下，可執行檔的目標架構是該 AppDomain 中的所有組件將在其下運行的相容性模式。

## <a name="runtime-changes"></a>執行階段變更

執行階段問題是在電腦上進行新的執行階段，而相同的二進位檔案也在執行時，會看到不同的行為。 如果二進位檔已針對 .NET Framework 4.0 編譯，它就可以在 4.5 或更新版本中以 .NET Framework 4.0 相容性模式執行。 許多影響 4.5 的變更不會影響針對 4.0 所編譯的二進位檔。 這是 AppDomain 特有的功能，視項目組件的設定而定。

## <a name="retargeting-changes"></a>重定目標變更

重定目標問題，是當以 4.0 為目標的組件現在設定為以 4.5 為目標時發生的問題。 現在，組件選擇加入新功能並成為可能的舊功能相容性問題。 這同樣受項目組件支配，所以主控台應用程式會使用組件，或網站會參考組件。

## <a name="net-compatibility-diagnostics"></a>.NET 相容性診斷

.NET 相容性診斷是由 Roslyn 提供的分析器，可協助您識別各版 .NET Framework 之間的應用程式相容性問題。 此清單包含所有可用的分析器，但只有一部分會套用至任何特定的移轉。 分析器會判斷適用於規劃之移轉的問題，並且只會呈現這些問題。

每個問題包含下列資訊：

- 舊版中已變更的內容描述。

- 變更如何影響客戶，以及是否有任何因應措施可保留版本間的相容性。

- 變更的重要性評估。 應用程式相容性問題可分類如下：

    |   |   |
    |---|---|
    |主要|影響大量應用程式或需要大幅修改程式碼的重大變更。|
    |次要|影響少量應用程式或需要稍微修改程式碼的變更。|
    |邊緣案例|在非常特定 (罕見) 的情況下影響應用程式的變更。|
    |透明|對應用程式的開發人員或使用者影響的不明顯變更。|

- 第一次出現此變更的 Framework 版本。 也指出某些變更會引入到特定版本，然後在更新版本中還原。

- 變更類型：

    |   |   |
    |---|---|
    |正在重定目標|此變更會影響為了以新版 .NET Framework 為目標而重新編譯的應用程式。|
    |執行階段|此變更會影響以舊版 .NET Framework 為目標但在新版上執行的現有應用程式。|

- 受影響的 API (如果有的話)。

- 可用診斷的識別碼

## <a name="usage"></a>使用量
若要開始，請選取以下的相容性變更類型：

- [重定目標變更](./retargeting/index.md)
- [執行階段變更](./runtime/index.md)

## <a name="see-also"></a>另請參閱

- [版本和相依性](versions-and-dependencies.md)
- [新功能](../whats-new/index.md)
- [類別庫中已淘汰的功能](../whats-new/whats-obsolete.md)
