---
title: ICorDebugProcess6::DecodeEvent 方法
ms.date: 03/30/2017
ms.assetid: 1453bc0c-6e0d-4d5a-b176-22607f8a3e6c
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: f81c513447b7c63fb16ff20ae6f83c3e6ef359b1
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2019
ms.locfileid: "69964048"
---
# <a name="icordebugprocess6decodeevent-method"></a>ICorDebugProcess6::DecodeEvent 方法
對已封裝在特殊設計之原生例外狀況偵錯事件承載中的 Managed 偵錯事件進行解碼。  
  
## <a name="syntax"></a>語法  
  
```cpp  
HRESULT DecodeEvent(  
        [in, length_is(countBytes), size_is(countBytes)]  const BYTE pRecord[],  
        [in] DWORD countBytes,  
        [in] CorDebugRecordFormat format,  
        [in] DWORD dwFlags,   
        [in] DWORD dwThreadId,   
        [out] ICorDebugDebugEvent **ppEvent  
);  
```  
  
## <a name="parameters"></a>參數  
 `pRecord`  
 [in] 原生例外狀況偵錯事件中的位元組陣列指標，該事件包含 Managed 偵錯事件的相關資訊。  
  
 `countBytes`  
 [in] `pRecord` 位元組陣列中的項目數。  
  
 `format`  
 在指定非受控 debug 事件格式的[CorDebugRecordFormat](../../../../docs/framework/unmanaged-api/debugging/cordebugrecordformat-enumeration.md)列舉成員。  
  
 `dwFlags`  
 [in] 相依於目標架構並指定偵錯事件之其他資訊的位元欄位。 對於 Windows 系統, 它可以是[CorDebugDecodeEventFlagsWindows](../../../../docs/framework/unmanaged-api/debugging/cordebugdecodeeventflagswindows-enumeration.md)列舉的成員。  
  
 `dwThreadId`  
 [in] 擲回例外狀況之執行緒的作業系統識別項。  
  
 `ppEvent`  
 脫銷[ICorDebugDebugEvent](../../../../docs/framework/unmanaged-api/debugging/icordebugdebugevent-interface.md)物件位址的指標, 表示已解碼的 managed 偵錯工具事件。  
  
## <a name="remarks"></a>備註  
  
> [!NOTE]
> 這個方法僅適用於 .NET Native。  
  
## <a name="requirements"></a>需求  
 **平台：** 請參閱[系統需求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **標頭：** CorDebug.idl、CorDebug.h  
  
 **LIBRARY:** CorGuids.lib  
  
 **.NET framework 版本：** [!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另請參閱

- [ICorDebugProcess6 介面](../../../../docs/framework/unmanaged-api/debugging/icordebugprocess6-interface.md)
- [偵錯介面](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)
