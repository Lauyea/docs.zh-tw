---
title: CorDebugStepReason 列舉
ms.date: 03/30/2017
api_name:
- CorDebugStepReason
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugStepReason
helpviewer_keywords:
- CorDebugStepReason enumeration [.NET Framework debugging]
ms.assetid: fe248069-b33c-48e1-a777-06ac9b239c54
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 3d9dc94689083d79858319387747eb9dafe8b2f6
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67739570"
---
# <a name="cordebugstepreason-enumeration"></a>CorDebugStepReason 列舉
指出個別步驟的結果。  
  
## <a name="syntax"></a>語法  
  
```cpp  
typedef enum CorDebugStepReason {  
    STEP_NORMAL,  
    STEP_RETURN,  
    STEP_CALL,  
    STEP_EXCEPTION_FILTER,  
    STEP_EXCEPTION_HANDLER,  
    STEP_INTERCEPT,  
    STEP_EXIT  
} CorDebugStepReason;  
```  
  
## <a name="members"></a>成員  
  
|成員|描述|  
|------------|-----------------|  
|`STEP_NORMAL`|逐步執行正常，完成相同的函式內。|  
|`STEP_RETURN`|逐步執行正常繼續進行之後的函式傳回。|  
|`STEP_CALL`|逐步執行正常繼續進行，在新的呼叫的函式的開頭。|  
|`STEP_EXCEPTION_FILTER`|產生例外狀況和控制項傳遞至例外狀況篩選條件。|  
|`STEP_EXCEPTION_HANDLER`|產生例外狀況和控制項傳遞至例外狀況處理常式。|  
|`STEP_INTERCEPT`|控制項傳遞至攔截器。|  
|`STEP_EXIT`|執行緒結束之前已完成該步驟。|  
  
## <a name="requirements"></a>需求  
 **平台：** 請參閱[系統需求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **標頭：** CorDebug.idl、CorDebug.h  
  
 **LIBRARY:** CorGuids.lib  
  
 **.NET framework 版本：** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另請參閱

- [StepComplete 方法](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-stepcomplete-method.md)
- [偵錯列舉](../../../../docs/framework/unmanaged-api/debugging/debugging-enumerations.md)
