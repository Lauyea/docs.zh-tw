---
title: INotifySink2::OnSyncCallExit 方法
ms.date: 03/30/2017
api_name:
- INotifySink2.OnSyncCallExit
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- INotifySink2::OnSyncCallExit
helpviewer_keywords:
- INotifySink2::OnSyncCallExit method [.NET Framework debugging]
- OnSyncCallExit method [.NET Framework debugging]
ms.assetid: d9d7600e-a8f5-443a-96de-67d26e130f2d
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 7c4932828669e61f14827934bacfec2ca0153b50
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67744515"
---
# <a name="inotifysink2onsynccallexit-method"></a>INotifySink2::OnSyncCallExit 方法
結束呼叫時叫用。  
  
## <a name="syntax"></a>語法  
  
```cpp  
HRESULT OnSyncCallExit  
(  
    [in]  CALL_ID   in_CallID,  
    [out, size_is(, *out_pBufferSize)] BYTE**  out_ppBuffer,  
    [out] DWORD*    out_pBufferSize  
);  
```  
  
## <a name="parameters"></a>參數  
 `in_CallID`  
 [in]正在結束的呼叫識別碼。 請參閱[CALL_ID 結構](../../../../docs/framework/unmanaged-api/diagnostics/call-id-structure.md)。  
  
 `out_ppBuffer`  
 [out]呼叫的緩衝區。  
  
 `out_pBufferSize`  
 [out]呼叫緩衝區，以位元組為單位的大小。  
  
## <a name="return-value"></a>傳回值  
 如果方法成功為 S_OK。  
  
## <a name="requirements"></a>需求  
 **標頭：** ProtocolNotify2.idl  
  
## <a name="see-also"></a>另請參閱

- [INotifySink2 介面](../../../../docs/framework/unmanaged-api/diagnostics/inotifysink2-interface.md)
- [INotifySource2 介面](../../../../docs/framework/unmanaged-api/diagnostics/inotifysource2-interface.md)
- [INotifyConnection2 介面](../../../../docs/framework/unmanaged-api/diagnostics/inotifyconnection2-interface.md)
