---
title: 堆疊 ETW 事件
ms.date: 03/30/2017
helpviewer_keywords:
- stack event [.NET Framework]
- ETW, stack event (CLR)
ms.assetid: f612fa5b-4b62-4593-a19e-85c9b1018dce
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 5dc23f5105b589d5b74c9ea6b7f40b84c2b04e6a
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2019
ms.locfileid: "71046160"
---
# <a name="stack-etw-event"></a>堆疊 ETW 事件
堆疊事件應該搭配其他事件一起使用，以在引發事件之後產生堆疊追蹤。 它會在啟用執行階段提供者時記錄。 這是非常高頻率的事件，因為每當引發另一個執行階段事件時，就會引發此事件。 基於這個理由，我們建議您小心使用此事件。  
  
 下表說明關鍵字和層級。 (如需詳細資訊，請參閱 [CLR ETW Keywords and Levels](clr-etw-keywords-and-levels.md))。  
  
|引發事件的關鍵字|層級|  
|-----------------------------------|-----------|  
|`StackKeyword` (0x40000000)|LogAlways(0)|  
  
 下表說明事件資訊。  
  
|Event - 事件|事件 ID|引發的時機|  
|-----------|--------------|-----------------|  
|`CLRStackWalk`|82|搭配其他事件，來產生事件之後的堆疊追蹤。|  
  
 下表說明事件資料。  
  
|欄位名稱|資料類型|說明|  
|----------------|---------------|-----------------|  
|ClrInstanceID|win:Uint16|唯一執行階段識別項。|  
|Reserved1|win:UInt8|保留的。|  
|Reserved2|win:UInt8|保留的。|  
|FrameCount|win:UInt32|堆疊追蹤裡的框架數。|  
|堆疊|win:Pointer|指令指標的資料行。|  
  
## <a name="see-also"></a>另請參閱

- [CLR ETW 事件](clr-etw-events.md)
