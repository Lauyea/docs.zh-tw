---
title: <Event>元素（.NET Native）
ms.date: 03/30/2017
ms.assetid: e53b029c-9d6d-4c0a-9cdc-5cfca8a5ca47
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 7fb0245c50677da0397ba9c4918f171dcb217ba6
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2019
ms.locfileid: "71049852"
---
# <a name="event-element-net-native"></a>\<Event > 元素（.NET Native）
將執行階段反映原則套用至事件。  
  
## <a name="syntax"></a>語法  
  
```xml  
<Event Name="event_name"   
       Browse="policy_type"   
       Dynamic="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>屬性和項目  
 下列各節描述屬性、子項目和父項目。  
  
### <a name="attributes"></a>屬性  
  
|屬性|屬性類型|描述|  
|---------------|--------------------|-----------------|  
|`Name`|一般|必要屬性。 指定事件名稱。|  
|`Browse`|反射|選擇性屬性。 控制事件相關資訊的查詢或事件的列舉，但不會在執行階段啟用任何動態存取。|  
|`Dynamic`|反射|選擇性屬性。 控制事件的執行階段存取，以便啟用動態程式設計。 這個原則可確保在執行階段能夠以動態方式來處理事件。|  
  
## <a name="name-attribute"></a>Name 屬性  
  
|值|說明|  
|-----------|-----------------|  
|*method_name*|事件名稱。 事件的類型是由父 [\<Type>](type-element-net-native.md) 或 [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 項目所定義。|  
  
## <a name="all-other-attributes"></a>所有其他屬性  
  
|值|說明|  
|-----------|-----------------|  
|*policy_setting*|要套用至事件之這個原則類型的設定。 可能的值為 `Auto`、`Excluded`、`Included` 和 `Required`。 如需詳細資訊，請參閱[執行階段指示詞原則設定](runtime-directive-policy-settings.md)。|  
  
### <a name="child-elements"></a>子元素  
 無。  
  
### <a name="parent-elements"></a>父項目  
  
|項目|描述|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|將反映原則套用至類型及其所有成員。|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|將反映原則套用至建構泛型類型及其所有成員。|  
  
## <a name="remarks"></a>備註  
 如果未明確定義事件的原則，則會繼承其父項目的執行階段原則。  
  
## <a name="see-also"></a>另請參閱

- [執行階段指示詞 (rd.xml) 組態檔參考](runtime-directives-rd-xml-configuration-file-reference.md)
- [執行階段指示詞項目](runtime-directive-elements.md)
- [執行階段指示詞原則設定](runtime-directive-policy-settings.md)
