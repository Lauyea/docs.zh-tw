---
title: <nameEntry> 項目
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#nameEntry
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/mscorlib/cryptographySettings/cryptoNameMapping/nameEntry
helpviewer_keywords:
- <nameEntry> element
- nameEntry element
ms.assetid: 7d7535e9-4b4a-4b8c-82e2-e40dff5a7821
ms.openlocfilehash: a339638587f8b544bbc1b0073553f6232ce09694
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71699785"
---
# <a name="nameentry-element"></a>@no__t 0nameEntry > 元素
將類別名稱對應至易記的演算法名稱，允許一個類別有許多易記名稱。  
  
[ **\<configuration>** ](../configuration-element.md)  
&nbsp; @ no__t-1[ **\<mscorlib >** ](mscorlib-element-for-cryptography-settings.md)  
&nbsp; @ no__t-1 @ no__t-2 @ no__t-3[ **\<cryptographySettings >** ](cryptographysettings-element.md)  
&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5[ **\<cryptoNameMapping >** ](cryptonamemapping-element.md)  
&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 **\<nameEntry >**  
  
## <a name="syntax"></a>語法  
  
```xml  
<nameEntry name="friendly name" Class="class name" />  
```  
  
## <a name="attributes-and-elements"></a>屬性和項目  
 下列各節描述屬性、子項目和父項目。  
  
### <a name="attributes"></a>屬性  
  
|屬性|描述|  
|---------------|-----------------|  
|**name**|必要屬性。<br /><br /> 指定密碼編譯類別所實行之演算法的易記名稱。|  
|**class**|必要屬性。<br /><br /> 指定[@no__t 2cryptoClass >](cryptoclass-element.md)元素中**name**屬性的值。|  
  
### <a name="child-elements"></a>子元素  
 無。  
  
### <a name="parent-elements"></a>父項目  
  
|項目|描述|  
|-------------|-----------------|  
|`configuration`|通用語言執行平台和 .NET Framework 應用程式所使用之每個組態檔中的根項目。|  
|`system.web`|指定 ASP.NET 組態區段的根項目。|  
  
## <a name="remarks"></a>備註  
 **Name**屬性可以是在 @no__t 1 命名空間中找到的其中一個抽象類別的名稱。 當您在抽象密碼編譯類別上呼叫**Create**方法時，會將抽象類別名稱傳遞給 <xref:System.Security.Cryptography.CryptoConfig.CreateFromName%2A> 方法。 **CreateFromName**會傳回**類別**屬性所指示之類型的實例。 如果**name**屬性是簡短名稱（例如 RSA），您可以在呼叫**CreateFromName**方法時使用該名稱。  
  
## <a name="example"></a>範例  
 下列範例示範如何使用 **@no__t 1nameEntry 的 >** 專案來參考密碼編譯類別，並設定執行時間。 接著，您可以將字串 "RSA" 傳遞給 <xref:System.Security.Cryptography.CryptoConfig.CreateFromName%2A?displayProperty=nameWithType> 方法，然後使用 <xref:System.Security.Cryptography.AsymmetricAlgorithm.Create%2A> 方法傳回 @no__t 2 物件。  
  
```xml  
<configuration>  
   <mscorlib>  
      <cryptographySettings>  
         <cryptoNameMapping>  
            <cryptoClasses>  
               <cryptoClass   MyCryptoRSA="MyCryptoRSAClass, MyAssembly  
                  Culture=neutral, PublicKeyToken=a5d015c7d5a0b012,  
                  Version=1.0.0.0"/>  
            </cryptoClasses>  
            <nameEntry name="RSA" class="MyCryptoRSA"/>  
            <nameEntry name="System.Security.Cryptography.AsymmetricAlgorithm"  
                       class="MyCryptoRSA"/>  
         </cryptoNameMapping>  
      </cryptographySettings>  
   </mscorlib>  
</configuration>  
```  
  
## <a name="see-also"></a>另請參閱

- [組態檔結構描述](../index.md)
- [密碼編譯設定結構描述](index.md)
- [The signature is valid](../../../../standard/security/cryptographic-services.md)
- [設定密碼編譯類別](../../configure-cryptography-classes.md)
