---
title: EContextType 列舉
ms.date: 03/30/2017
api_name:
- EContextType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- EContextType
helpviewer_keywords:
- EContextType enumeration [.NET Framework hosting]
ms.assetid: 92b926a9-b87e-408a-9036-df7b752c9492
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: f93f36a78ff5579e131ef4bb3d48f04e806c14de
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67779389"
---
# <a name="econtexttype-enumeration"></a>EContextType 列舉
描述目前執行中執行緒的安全性內容。  
  
## <a name="syntax"></a>語法  
  
```cpp  
typedef enum {  
    eCurrentContext    = 0x00,  
    eRestrictedContext = 0x01  
} EContextType;  
```  
  
## <a name="members"></a>成員  
  
|成員|描述|  
|------------|-----------------|  
|`eCurrentContext`|目前的執行緒上的內容會指出在 common language runtime (CLR) 呼叫的時[ihostsecuritymanager:: Getsecuritycontext](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritymanager-getsecuritycontext-method.md)方法或呼叫中的 CLR 所要求的內容[Ihostsecuritymanager:: Setsecuritycontext](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritymanager-setsecuritycontext-method.md)方法。|  
|`eRestrictedContext`|表示透過主應用程式具有較低的權限，例如記憶體回收行程或類別或模組的建構函式的內容。|  
  
## <a name="remarks"></a>備註  
 CLR 提供的其中一個`EContextType`呼叫的參數值的值`IHostSecurityManager::GetSecurityContext`和`IHostSecurityManager::SetSecurityContext`方法。  
  
## <a name="requirements"></a>需求  
 **平台：** 請參閱[系統需求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **標頭：** MSCorEE.h  
  
 **LIBRARY:** MSCorEE.dll  
  
 **.NET framework 版本：** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另請參閱

- [IHostSecurityContext 介面](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritycontext-interface.md)
- [IHostSecurityManager 介面](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritymanager-interface.md)
- [裝載列舉](../../../../docs/framework/unmanaged-api/hosting/hosting-enumerations.md)
