---
title: AXL_AUTHENTICODE_SIGNER_INFO 結構
ms.date: 03/30/2017
ms.assetid: 81c0f8b4-ce35-4716-8651-b642d40648a2
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 9aaa0258b53b6b39874c8c99c71ecf53cbdb8f97
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70787073"
---
# <a name="axl_authenticode_signer_info-structure"></a>AXL_AUTHENTICODE_SIGNER_INFO 結構
定義 Authenticode 簽署人資訊。  
  
## <a name="syntax"></a>語法  
  
```cpp  
typedef struct _AXL_AUTHENTICODE_SIGNER_INFO {  
    DWORD cbSize;  
    HRESULT dwError;  
    ALG_ID algHash;  
    LPCWSTR pwszHash  
    LPCWSTR pwszDescription;  
    LPCWSTR pwszDescriptionUrl;  
    PCCERT_CHAIN_CONTEXT pChainContext  
} AXL_AUTHENTICODE_SIGNER_INFO, * PAXL_AUTHENTICODE_SIGNER_INFO;  
```  
  
## <a name="members"></a>成員  
  
|成員|描述|  
|------------|-----------------|  
|`cbSize`|此結構的大小。|  
|`dwError`|錯誤碼。|  
|`algHash`|雜湊演算法。|  
|`pwszHash`|雜湊。|  
|`pwszDescription`|描述。|  
|`pwszDescriptionUrl`|描述的 URL。|  
|`pChainContext`|簽署人的鏈結內容。 請參閱[CERT_CONTEXT](/windows/win32/api/wincrypt/ns-wincrypt-cert_context)結構。|  
  
## <a name="see-also"></a>另請參閱

- [Authenticode](index.md)
