---
title: <Field>元素（.NET Native）
ms.date: 03/30/2017
ms.assetid: 6a14125f-1a8d-41a1-8a32-659ca0ad12de
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 6dfb6a07f9733ab1a01a1ce9917c6a4bb4ce793b
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2019
ms.locfileid: "71049769"
---
# <a name="field-element-net-native"></a>\<Field > 元素（.NET Native）
將執行階段反映原則套用至欄位。  
  
## <a name="syntax"></a>語法  
  
```xml  
<Field Name="field_name"  
       Browse="policy_type"  
       Dynamic="policy_type"  
       Serialize="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>屬性和項目  
 下列各節描述屬性、子項目和父項目。  
  
### <a name="attributes"></a>屬性  
  
|屬性|屬性類型|說明|  
|---------------|--------------------|-----------------|  
|`Name`|一般|必要屬性。 指定欄位名稱。|  
|`Browse`|反射|選擇性屬性。 控制對欄位相關資訊的查詢，或控制欄位的列舉，但不會在執行階段啟用任何動態存取。|  
|`Dynamic`|反射|選擇性屬性。 控制對欄位的執行階段存取權，以啟用動態程式設計。 此原則可確保能夠在執行階段動態設定或擷取欄位。|  
|`Serialize`|序列化|選擇性屬性。 控制對欄位的執行階段存取權，使類型執行個體能夠由 Newtonsoft JSON 序列化程式之類的程式庫來序列化，或是用於資料繫結。|  
  
## <a name="name-attribute"></a>Name 屬性  
  
|值|描述|  
|-----------|-----------------|  
|*method_name*|欄位名稱。 欄位的類型是由父系 [\<Type>](type-element-net-native.md) 或 [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 項目定義。|  
  
## <a name="all-other-attributes"></a>所有其他屬性  
  
|值|說明|  
|-----------|-----------------|  
|*policy_setting*|要為欄位套用此原則類型的設定。 可能的值為 `Auto`、`Excluded`、`Included` 和 `Required`。 如需詳細資訊，請參閱[執行階段指示詞原則設定](runtime-directive-policy-settings.md)。|  
  
### <a name="child-elements"></a>子元素  
 無。  
  
### <a name="parent-elements"></a>父項目  
  
|項目|說明|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|將反映原則套用至類型及其所有成員。|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|將反映原則套用至建構泛型類型及其所有成員。|  
  
## <a name="remarks"></a>備註  
 如果未明確定義欄位的原則，則會繼承其父元素的執行階段原則。  
  
## <a name="see-also"></a>另請參閱

- [執行階段指示詞項目](runtime-directive-elements.md)
- [執行階段指示詞 (rd.xml) 組態檔參考](runtime-directives-rd-xml-configuration-file-reference.md)
- [執行階段指示詞原則設定](runtime-directive-policy-settings.md)
