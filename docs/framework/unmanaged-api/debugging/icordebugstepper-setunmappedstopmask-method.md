---
title: ICorDebugStepper::SetUnmappedStopMask 方法
ms.date: 03/30/2017
api_name:
- ICorDebugStepper.SetUnmappedStopMask
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugStepper::SetUnmappedStopMask
helpviewer_keywords:
- ICorDebugStepper::SetUnmappedStopMask method [.NET Framework debugging]
- SetUnmappedStopMask method [.NET Framework debugging]
ms.assetid: b1211981-e90c-4e05-8def-fa18d85ad9ab
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: c0a273c54559e8e297e09740ba9c770ce12d72d1
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67760579"
---
# <a name="icordebugsteppersetunmappedstopmask-method"></a>ICorDebugStepper::SetUnmappedStopMask 方法
設定值，這個值會指定未對應的程式碼執行將會暫止的類型。  
  
## <a name="syntax"></a>語法  
  
```cpp  
HRESULT SetUnmappedStopMask (  
    [in] CorDebugUnmappedStop   mask  
);  
```  
  
## <a name="parameters"></a>參數  
 `mask`  
 [in]CorDebugUnmappedStop 列舉，指定未對應的程式碼中偵錯工具將會暫止執行的類型值。  
  
 預設值是 STOP_OTHER_UNMAPPED。 值 STOP_UNMANAGED 只適用於 interop 偵錯。  
  
## <a name="remarks"></a>備註  
 如果您已設定旗標，指定該類型的未對應的程式碼; 時偵錯工具會尋找在 just-in-time (JIT) 編譯成 Microsoft intermediate language (MSIL) 沒有對應的對應，請它先終止執行否則，繼續逐步執行以透明的方式。  
  
 如果偵錯工具不使用步進輸入方法，則它不一定不進入未對應的程式碼。  
  
## <a name="requirements"></a>需求  
 **平台：** 請參閱[系統需求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **標頭：** CorDebug.idl、CorDebug.h  
  
 **LIBRARY:** CorGuids.lib  
  
 **.NET framework 版本：** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
