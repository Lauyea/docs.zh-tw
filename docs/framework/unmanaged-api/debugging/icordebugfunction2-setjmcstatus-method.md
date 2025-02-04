---
title: ICorDebugFunction2::SetJMCStatus 方法
ms.date: 03/30/2017
api_name:
- ICorDebugFunction2.SetJMCStatus
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugFunction2::SetJMCStatus
helpviewer_keywords:
- ICorDebugFunction2::SetJMCStatus method [.NET Framework debugging]
- SetJMCStatus method, ICorDebugFunction2 interface [.NET Framework debugging]
ms.assetid: 22c27b01-2869-4214-b840-5921f7c874fc
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 67959b2ebbfb62b47a1b2a770e278d043fc66d21
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67754926"
---
# <a name="icordebugfunction2setjmcstatus-method"></a>ICorDebugFunction2::SetJMCStatus 方法
表示此 ICorDebugFunction2 Just My Code 的函式會將標示逐步執行。  
  
## <a name="syntax"></a>語法  
  
```cpp  
HRESULT SetJMCStatus (  
    [in] BOOL   bIsJustMyCode  
);  
```  
  
## <a name="parameters"></a>參數  
 `bIsJustMyCode`  
 [in]設定為`true`來標記的功能與使用者程式碼; 否則設定為`false`。  
  
## <a name="return-values"></a>傳回值  
  
|HRESULT|描述|  
|-------------|-----------------|  
|`S_OK`|已成功地標示函式。|  
|`CORDBG_E_FUNCTION_NOT_DEBUGGABLE`|此函式無法標示為使用者程式碼，因為它不進行偵錯。|  
  
## <a name="remarks"></a>備註  
 Just My Code 步進會略過的非使用者程式碼。 使用者程式碼必須是可偵錯的程式碼的子集。  
  
## <a name="requirements"></a>需求  
 **平台：** 請參閱[系統需求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **標頭：** CorDebug.idl、CorDebug.h  
  
 **LIBRARY:** CorGuids.lib  
  
 **.NET framework 版本：** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
