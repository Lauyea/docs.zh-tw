---
title: <httpTransport>
ms.date: 03/30/2017
ms.assetid: 8b30c065-b32a-4fa3-8eb4-5537a9c6b897
ms.openlocfilehash: 51558a7f51ddeab4652abcc72376cb50a22c239b
ms.sourcegitcommit: 093571de904fc7979e85ef3c048547d0accb1d8a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70400377"
---
# <a name="httptransport"></a>\<httpTransport>
指定 HTTP 傳輸，以傳輸自訂繫結的 SOAP 訊息。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<System.servicemodel >** ](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<系結 >** ](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<customBinding >** ](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<系結 >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<HTTPTransport >**  
  
## <a name="syntax"></a>語法  
  
```xml  
<httpTransport allowCookies="Boolean"
               authenticationScheme="Digest/Negotiate/Ntlm/Basic/Anonymous"
               bypassProxyOnLocal="Boolean"
               hostnameComparisonMode="StrongWildcard/Exact/WeakWildcard"
               keepAliveEnabled="Boolean"
               maxBufferSize="Integer"
               proxyAddress="Uri"
               proxyAuthenticationScheme="None/Digest/Negotiate/Ntlm/Basic/Anonymous"
               realm="String"
               transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse"
               unsafeConnectionNtlmAuthentication="Boolean"
               useDefaultWebProxy="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>屬性和項目  
 下列各節描述屬性、子項目和父項目。  
  
### <a name="attributes"></a>屬性  
  
|屬性|描述|  
|---------------|-----------------|  
|allowCookies|布林值，指定用戶端是否接受 Cookie 並在未來要求時傳播 Cookie。 預設為 `false`。<br /><br /> 當您與使用 Cookie 的 ASMX Web 服務互動時，可以使用這個屬性。 如此一來，從伺服器傳回的 Cookie 就一定會自動複製到該服務未來所有的用戶端要求。|  
|authenticationScheme|指定通訊協定，用於驗證由 HTTP 接聽程式處理的用戶端要求。 有效值包括以下的值：<br /><br /> 希指定摘要式驗證。<br />Negotiate與用戶端協商以判斷驗證配置。 如果用戶端和伺服器都支援 Kerberos，就使用它，否則使用 NTLM。<br />Ntlm指定 NTLM 驗證。<br />基本指定基本驗證。<br />匿名指定匿名驗證。<br /><br /> 預設值為 Anonymous。 此屬性的型別為 <xref:System.Net.AuthenticationSchemes>。 這個屬性只可以設定一次。|  
|bypassProxyOnLocal|布林值，指出本機位址是否略過 Proxy 伺服器。 預設為 `false`。<br /><br /> 本機位址是位於本機 LAN 或內部網路上的位址。<br /><br /> Windows Communication Foundation (WCF) 一律忽略 proxy，如果服務位址開頭 `http://localhost` 。<br /><br /> 如果您希望用戶端在與相同電腦上的服務進行交談時通過 Proxy，應使用主機名稱而非 localhost。|  
|hostnameComparisonMode|指定用於剖析 URI 的 HTTP 主機名稱比較模式。 有效值為：<br /><br /> -StrongWildcard：（"+"）會比對指定之配置、埠和相對 URI 內容中的所有可能主機名稱。<br />-Exact：不含萬用字元<br />-WeakWildcard：（"\*"）會比對指定之配置、埠和相對 UIR 內容中未明確符合或透過強式萬用字元機制的所有可能主機名稱。<br /><br /> 此屬性的型別為 <xref:System.ServiceModel.HostNameComparisonMode>。 預設為 <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard>。|  
|keepAliveEnabled|布林值，指定是否要與網際網路資源建立持續連線。|  
|maxBufferSize|正整數，指定緩衝區的大小上限。 預設為 524288。|  
|proxyAddress|指定 HTTP Proxy 位址的 URI。 如果 `useSystemWebProxy` 為 `true`，則這項設定必須為 `null`。 預設為 `null`。|  
|proxyAuthenticationScheme|指定通訊協定，用於驗證由 HTTP Proxy 處理的用戶端要求。 有效值包括以下的值：<br /><br /> 無不會執行任何驗證。<br />希指定摘要式驗證。<br />Negotiate與用戶端協商以判斷驗證配置。 如果用戶端和伺服器都支援 Kerberos，就使用它，否則使用 NTLM。<br />Ntlm指定 NTLM 驗證。<br />基本指定基本驗證。<br />匿名指定匿名驗證。<br /><br /> 預設值為 Anonymous。 此屬性的型別為 <xref:System.Net.AuthenticationSchemes>。 請注意<xref:System.Net.AuthenticationSchemes.IntegratedWindowsAuthentication?displayProperty=nameWithType> ，不受支援。|  
|realm|字串，指定在 Proxy/伺服器上使用的領域。 預設值是空字串。<br /><br /> 伺服器使用領域來分割受保護的資源。 每個分割都可以有自己的驗證配置和 (或) 授權資料庫。 領域只限於基本和摘要式驗證使用。 當用戶端成功驗證之後，驗證對指定領域中的所有資源都有效。 如需領域的詳細說明，請參閱[IETF 網站](https://www.ietf.org)的 RFC 2617。|  
|transferMode|指定訊息是否要經過緩衝處理或資料流處理，或為要求或回應。 有效值包括以下的值：<br /><br /> 緩衝要求和回應訊息會經過緩衝處理。<br />指要求和回應訊息會進行資料流程處理。<br />StreamedRequest資料流處理要求訊息，緩衝處理回應訊息。<br />- StreamedResponse:緩衝處理要求訊息，資料流處理回應訊息。<br /><br /> 預設為 Buffered。 此屬性的型別為 <xref:System.ServiceModel.TransferMode>。|  
|unsafeConnectionNtlmAuthentication|布林值，指定是否已在伺服器啟用「不安全的連線共用」。 預設為 `false`。 如果已啟用，NTLM 驗證會在各 TCP 連線上執行一次。|  
|useDefaultWebProxy|布林值，指定是否使用整部機器 Proxy 設定而非使用者特定設定。 預設為 `true`。|  
  
### <a name="child-elements"></a>子元素  
 None  
  
### <a name="parent-elements"></a>父項目  
  
|項目|描述|  
|-------------|-----------------|  
|[\<binding>](../../../misc/binding.md)|定義自訂繫結的所有繫結功能。|  
  
## <a name="remarks"></a>備註  
 `httpTransport` 項目是在建立自訂繫結時的起點，該繫結會實作 HTTP 傳輸通訊協定。 HTTP 是用於互通性目的的主要傳輸。 Windows Communication Foundation （WCF）支援此傳輸，以確保與其他非 WCF Web 服務堆疊之間的互通性。  
  
## <a name="see-also"></a>另請參閱

- <xref:System.ServiceModel.Configuration.HttpTransportElement>
- <xref:System.ServiceModel.Channels.HttpTransportBindingElement>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [傳輸](../../../wcf/feature-details/transports.md)
- [選擇傳輸](../../../wcf/feature-details/choosing-a-transport.md)
- [繫結](../../../wcf/bindings.md)
- [擴充繫結](../../../wcf/extending/extending-bindings.md)
- [自訂繫結](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
