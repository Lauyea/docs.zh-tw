---
title: <issuedToken>
ms.date: 03/30/2017
ms.assetid: b6eae4b7-a6cd-4e1a-b0f6-f407022550b0
ms.openlocfilehash: b5ab3c3ad070499d686ea74b9fd459e89f380cfa
ms.sourcegitcommit: 093571de904fc7979e85ef3c048547d0accb1d8a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70397968"
---
# <a name="issuedtoken"></a>\<issuedToken>
指定用來向服務驗證用戶端的自訂權杖。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<System.servicemodel >** ](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<行為 >** ](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<endpointBehaviors >** ](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<行為 >** ](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<clientCredentials >** ](clientcredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<issuedToken >**  
  
## <a name="syntax"></a>語法  
  
```xml  
<issuedToken cacheIssuedTokens="Boolean"
             defaultKeyEntropyMode="ClientEntropy/ServerEntropy/CombinedEntropy"
             issuedTokenRenewalThresholdPercentage = "0 to 100"
             issuerChannelBehaviors="String"
             localIssuerChannelBehaviors="String"
             maxIssuedTokenCachingTime="TimeSpan">
</issuedToken>
```  
  
## <a name="attributes-and-elements"></a>屬性和項目  
 下列各節描述屬性、子項目和父項目。  
  
### <a name="attributes"></a>屬性  
  
|屬性|描述|  
|---------------|-----------------|  
|`cacheIssuedTokens`|選用性的布林值屬性，指定是否快取權杖。 預設為 `true`。|  
|`defaultKeyEntropyMode`|選用性字串屬性，這個屬性會指定交握作業要使用的亂數值 (Entropy)。 值包括 `ClientEntropy`、`ServerEntropy` 與 `CombinedEntropy`，預設為 `CombinedEntropy`。 此屬性的型別為 <xref:System.ServiceModel.Security.SecurityKeyEntropyMode>。|  
|`issuedTokenRenewalThresholdPercentage`|選用性的整數屬性，這個屬性會指定權杖更新前，可通過的有效時間範圍 (由權杖簽發者提供) 百分比。 值為 0 到 100。 預設為 60，表示嘗試更新前有 60% 的時間通過。|  
|`issuerChannelBehaviors`|選用性屬性，這個屬性會指定與簽發者通訊時所用的通道行為。|  
|`localIssuerChannelBehaviors`|選用性屬性，這個屬性會指定與本機簽發者通訊時所用的通道行為。|  
|`maxIssuedTokenCachingTime`|選用性的 Timespan 屬性，當權杖簽發者 (STS) 沒有指定時間時，指定快取發行的權杖之期間。 預設值為 "10675199.02：48： 05.4775807"。|  
  
### <a name="child-elements"></a>子元素  
  
|項目|描述|  
|-------------|-----------------|  
|[\<localIssuer>](localissuer.md)|指定權杖的本機簽發者位址，與用來與端點通訊的繫結。|  
|[\<issuerChannelBehaviors>](issuerchannelbehaviors-element.md)|指定連絡本機簽發者時要使用的端點行為。|  
  
### <a name="parent-elements"></a>父項目  
  
|項目|描述|  
|-------------|-----------------|  
|[\<clientCredentials>](clientcredentials.md)|指定用來對服務驗證用戶端的認證。|  
  
## <a name="remarks"></a>備註  
 發行的權杖會在某些情況下當做自訂認證型別使用，例如在聯合案例中與安全權杖服務 (STS) 進行驗證時。 根據預設，這個權杖是 SAML 權杖。 如需詳細資訊，請參閱[同盟和發行的權杖](../../../wcf/feature-details/federation-and-issued-tokens.md)。 和[同盟和發行的權杖](../../../wcf/feature-details/federation-and-issued-tokens.md)。  
  
 這個區段包含用以設定權杖之本機簽發者的項目，或搭配安全性權杖服務使用的行為。 如需設定用戶端使用本機簽發者的指示，請[參閱如何：設定本機簽發者](../../../wcf/feature-details/how-to-configure-a-local-issuer.md)。  
  
## <a name="see-also"></a>另請參閱

- <xref:System.ServiceModel.Configuration.IssuedTokenClientElement>
- <xref:System.ServiceModel.Configuration.ClientCredentialsElement>
- <xref:System.ServiceModel.Description.ClientCredentials>
- <xref:System.ServiceModel.Configuration.ClientCredentialsElement.IssuedToken%2A>
- <xref:System.ServiceModel.Description.ClientCredentials.IssuedToken%2A>
- <xref:System.ServiceModel.Security.IssuedTokenClientCredential>
- [安全性行為](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [保護服務和用戶端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [同盟與發行的權杖](../../../wcf/feature-details/federation-and-issued-tokens.md)
- [保護用戶端安全](../../../wcf/securing-clients.md)
- [如何：建立同盟用戶端](../../../wcf/feature-details/how-to-create-a-federated-client.md)
- [如何：設定本機簽發者](../../../wcf/feature-details/how-to-configure-a-local-issuer.md)
- [同盟與發行的權杖](../../../wcf/feature-details/federation-and-issued-tokens.md)
