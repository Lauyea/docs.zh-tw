---
title: <MethodInstantiation>元素（.NET Native）
ms.date: 03/30/2017
ms.assetid: a3355d78-2a88-4109-8521-830d7cae260a
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: f2c0354853e4725ba3e673fb9142c4a7a85d2121
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2019
ms.locfileid: "71049625"
---
# <a name="methodinstantiation-element-net-native"></a>\<MethodInstantiation > 元素（.NET Native）
將執行階段反映原則套用至建構的泛型方法。  
  
## <a name="syntax"></a>語法  
  
```xml  
<MethodInstantiation Name="method_name"  
                     Signature="method_signature"  
                     Arguments="method_arguments"  
                     Browse="policy_type"  
                     Dynamic="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>屬性和項目  
 下列各節描述屬性、子項目和父項目。  
  
### <a name="attributes"></a>屬性  
  
|屬性|屬性類型|描述|  
|---------------|--------------------|-----------------|  
|`Name`|一般|必要屬性。 指定方法名稱。|  
|`Signature`|一般|選擇性屬性。 指定方法的具名參數。 以逗號分隔多個具名參數。 `Signature` 屬性用來區別多載的方法。|  
|`Arguments`|一般|必要屬性。 指定泛型型別引數。 如果有多個引數存在，會以逗號分隔。|  
|`Browse`|反射|選擇性屬性。 控制對方法相關資訊的查詢，或控制方法的列舉，但不會在執行階段啟用任何動態引動過程。|  
|`Dynamic`|反射|選擇性屬性。 控制對建構函式或方法的執行階段存取權，以啟用動態程式設計。 此原則確保能夠在執行階段動態叫用成員。|  
  
## <a name="name-attribute"></a>Name 屬性  
  
|值|描述|  
|-----------|-----------------|  
|*method_name*|方法名稱。 方法的類型是由父 [\<Type>](type-element-net-native.md) 或 [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 項目所定義。|  
  
## <a name="signature-attribute"></a>簽章屬性  
  
|值|說明|  
|-----------|-----------------|  
|*method_signature*|指定方法的具名參數。 如果有多個參數存在，會以逗號分隔。|  
  
## <a name="arguments-attribute"></a>引數屬性  
  
|值|說明|  
|-----------|-----------------|  
|*method_arguments*|指定泛型型別引數。 如果有多個引數存在，會以逗號分隔。 每個引數都必須包含完整的類型名稱。|  
  
## <a name="all-other-attributes"></a>所有其他屬性  
  
|值|說明|  
|-----------|-----------------|  
|*policy_setting*|要為方法套用此原則類型的設定。 可能的值為 `Auto`、`Excluded`、`Included` 和 `Required`。 如需詳細資訊，請參閱[執行階段指示詞原則設定](runtime-directive-policy-settings.md)。|  
  
### <a name="child-elements"></a>子元素  
 無。  
  
### <a name="parent-elements"></a>父項目  
  
|項目|描述|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|將反映原則套用至類型及其所有成員。|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|將反映原則套用至建構泛型類型及其所有成員。|  
  
## <a name="remarks"></a>備註  
 `<MethodInstantiation>` 元素會覆寫其對應開放式泛型方法的執行階段反映原則。  
  
## <a name="see-also"></a>另請參閱

- [執行階段指示詞 (rd.xml) 組態檔參考](runtime-directives-rd-xml-configuration-file-reference.md)
- [執行階段指示詞項目](runtime-directive-elements.md)
- [執行階段指示詞原則設定](runtime-directive-policy-settings.md)
- [\<Method> 項目](method-element-net-native.md)
