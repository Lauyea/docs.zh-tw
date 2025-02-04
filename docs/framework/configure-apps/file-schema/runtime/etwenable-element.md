---
title: <etwEnable> 項目
ms.date: 03/30/2017
helpviewer_keywords:
- etwEnable element
- <etwEnable> element
ms.assetid: 29dde982-6d8b-4099-8867-ad0d7733f6dc
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: cb4d0ed5b33170c40aacb32bebbf1b59ca659be4
ms.sourcegitcommit: 4e2d355baba82814fa53efd6b8bbb45bfe054d11
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/04/2019
ms.locfileid: "70252610"
---
# <a name="etwenable-element"></a>\<etwEnable > 元素
指定是否為通用語言執行平台事件啟用 Windows 事件追蹤 (ETW)。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<執行時間 >** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<etwEnabled >**  
  
## <a name="syntax"></a>語法  
  
```xml  
<etwEnable enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>屬性和項目  
 下列各節描述屬性、子項目和父項目。  
  
### <a name="attributes"></a>屬性  
  
|屬性|描述|  
|---------------|-----------------|  
|enabled|必要屬性。<br /><br /> 指定是否應該啟用 ETW。|  
  
## <a name="enabled-attribute"></a>啟用屬性  
  
|值|描述|  
|-----------|-----------------|  
|true|啟用 ETW。 從 Windows Vista 和 Windows Server 2008 作業系統開始, 這是 Windows 版本的預設值。|  
|false|停用 ETW。 這是舊版 Windows 的預設值。|  
  
### <a name="child-elements"></a>子元素  
 無。  
  
### <a name="parent-elements"></a>父項目  
  
|項目|說明|  
|-------------|-----------------|  
|`configuration`|通用語言執行平台和 .NET Framework 應用程式所使用之每個組態檔中的根項目。|  
|`runtime`|包含有關組件繫結和記憶體回收的資訊。|  
  
## <a name="remarks"></a>備註  
 從 Windows Vista 開始, 預設會啟用 ETW。 使用此元素可停用應用程式的 ETW。 在舊版的 Windows 中, 請使用此元素來啟用應用程式的 ETW。  
  
> [!NOTE]
> 您可以使用登錄設定, 在伺服器上全域啟用或停用 ETW。 請參閱[控制 .NET Framework 記錄](../../../performance/controlling-logging.md)。  
  
## <a name="example"></a>範例  
 下列範例顯示如何啟用應用程式的 ETW 追蹤。  
  
```xml  
<configuration>  
   <runtime>  
      <etwEnable enabled="true" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>另請參閱

- [執行階段設定結構描述](index.md)
- [組態檔結構描述](../index.md)
- [控制 .NET Framework 記錄](../../../performance/controlling-logging.md)
