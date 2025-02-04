---
title: 風險降低：自訂 IMessageFilter.PreFilterMessage 實作
ms.date: 03/30/2017
ms.assetid: 9cf47c5b-0bb2-45df-9437-61cd7e7c2f4d
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 2af81468c5c4c4caf2f09725d6c7c4723084e35c
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70779426"
---
# <a name="mitigation-custom-imessagefilterprefiltermessage-implementations"></a>風險降低：自訂 IMessageFilter.PreFilterMessage 實作

在以 .NET Framework 4.6.1 和之後 .NET Framework 版本為目標的 Windows Forms 應用程式中，自訂的 <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage%2A?displayProperty=nameWithType> 實作可以在呼叫 <xref:System.Windows.Forms.Application.FilterMessage%2A?displayProperty=nameWithType> 方法時安全地篩選訊息 (如果 <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage%2A?displayProperty=nameWithType> 實作有下列狀況)：

- 執行下列其中一項或兩項：

  - 呼叫 <xref:System.Windows.Forms.Application.AddMessageFilter%2A> 方法來加入訊息篩選器。

  - 呼叫 <xref:System.Windows.Forms.Application.RemoveMessageFilter%2A> 方法來移除訊息篩選器。 方法。

- **並且**呼叫 <xref:System.Windows.Forms.Application.DoEvents%2A?displayProperty=nameWithType> 方法來激發訊息。

## <a name="impact"></a>影響

這項變更只會影響以 .NET Framework 4.6.1 和更新版本為目標的 Windows Forms 應用程式。

針對以舊版 .NET Framework 為目標的 Windows Forms 應用程式，在某些情況下這類實作會在呼叫 <xref:System.Windows.Forms.Application.FilterMessage%2A?displayProperty=nameWithType> 方法時擲回 <xref:System.IndexOutOfRangeException> 例外狀況

## <a name="mitigation"></a>緩和

如果這不是您要的變更，則以 .NET Framework 4.6.1 或更新版本為目標之應用程式可以藉由將下列組態設定新增至應用程式組態檔的[\<執行階段>](../configure-apps/file-schema/runtime/runtime-element.md) 區段，來選擇退出這項行為：

```xml
<runtime>
    <AppContextSwitchOverrides value="Switch.System.Windows.Forms.DontSupportReentrantFilterMessage=true" />
</runtime>
```

此外，以舊版 .NET Framework 為目標，但在 .NET Framework 4.6.1 或更新版本下執行的應用程式，可以藉由將下列組態設定新增至應用程式組態檔的[\<執行階段>](../configure-apps/file-schema/runtime/runtime-element.md) 區段，來選擇加入這項行為：

```xml
<runtime>
    <AppContextSwitchOverrides value="Switch.System.Windows.Forms.DontSupportReentrantFilterMessage=false" />
</runtime>
```

## <a name="see-also"></a>另請參閱

- [重定目標變更](retargeting-changes-in-the-net-framework-4-6-1.md)
