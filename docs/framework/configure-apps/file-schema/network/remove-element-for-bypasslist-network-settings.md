---
title: bypasslist 的 <remove> 項目 (網路設定)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/defaultProxy/bypasslist/remove
- http://schemas.microsoft.com/.NetConfiguration/v2.0#remove
helpviewer_keywords:
- <bypasslist>, remove element
- remove element, bypasslist
- bypasslist, remove element
- remove element, bypasslist
ms.assetid: 61dcfb4a-e3d9-4abf-a2cd-7d685fe2f64b
ms.openlocfilehash: 97b49a8a520d6a4f72945366874991d2deb18710
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71697899"
---
# <a name="remove-element-for-bypasslist-network-settings"></a>適用于 bypasslist 的 @no__t 0remove > 元素（網路設定）

從 proxy 略過清單中移除 IP 位址或 DNS 名稱。

[ **\<configuration>** ](../configuration-element.md)  
&nbsp; @ no__t-1[ **\<system. net >** ](system-net-element-network-settings.md)  
&nbsp; @ no__t-1 @ no__t-2 @ no__t-3[ **\<defaultProxy >** ](defaultproxy-element-network-settings.md)  
&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5[ **\<bypasslist >** ](bypasslist-element-network-settings.md)  
&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 **\<remove >**  

## <a name="syntax"></a>語法

```xml
<remove
  address="regular expression"
/>
```

## <a name="attributes-and-elements"></a>屬性和項目

下列各節描述屬性、子項目和父項目。

### <a name="attributes"></a>屬性

|**屬性**|**描述**|
|-------------------|---------------------|
|`address`|描述 IP 位址或 DNS 名稱的正則運算式。|

### <a name="child-elements"></a>子元素

無。

### <a name="parent-elements"></a>父項目

|**目**|**描述**|
|-----------------|---------------------|
|[bypasslist](bypasslist-element-network-settings.md)|提供一組正則運算式，描述不使用 proxy 的位址。|

## <a name="remarks"></a>備註

@No__t-0 元素會從略過 proxy 伺服器的地址清單中，移除描述 IP 位址或 DNS 伺服器名稱的正則運算式。 先前已在設定檔或設定階層的較高層級定義位址。

@No__t-0 屬性的值應該是描述一組 IP 位址或主機名稱的正則運算式。

如需正則運算式的詳細資訊，請參閱。[.NET Framework 正則運算式](../../../../standard/base-types/regular-expressions.md)。

## <a name="configuration-files"></a>組態檔

此項目可以用於應用程式組態檔或電腦組態檔 (Machine.config)。

## <a name="example"></a>範例

下列範例會移除 adventure-works.com 網域的任何先前定義，然後將 contoso.com 網域新增至略過清單。

```xml
<configuration>
  <system.net>
    <defaultProxy>
      <bypasslist>
        <remove address="[a-z]+\.adventure-works\.com$" />
        <add address="[a-z]+\.contoso\.com$" />
      </bypasslist>
    </defaultProxy>
  </system.net>
</configuration>
```

## <a name="see-also"></a>另請參閱

- <xref:System.Net.WebProxy?displayProperty=nameWithType>
- [網路設定結構描述](index.md)
