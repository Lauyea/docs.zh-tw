---
title: ICorProfilerInfo4::GetILToNativeMapping2 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4.GetILToNativeMapping2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4::GetILToNativeMapping2
helpviewer_keywords:
- ICorProfilerInfo4::GetILToNativeMapping2 method [.NET Framework profiling]
- GetILToNativeMapping2 method, ICorProfilerInfo4 interface [.NET Framework profiling]
ms.assetid: 756c1c25-08a7-4060-9798-dbeaa2f3bee5
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: a8ffdb04bdf3fd2f605e2dffc5065a0d786bbaf7
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2019
ms.locfileid: "69967909"
---
# <a name="icorprofilerinfo4getiltonativemapping2-method"></a>ICorProfilerInfo4::GetILToNativeMapping2 方法
針對指定函式的 JIT 重新編譯版本中所包含的程式碼，取得從 Microsoft Intermediate Language (MSIL) 位移到原生位移的對應。  
  
## <a name="syntax"></a>語法  
  
```cpp  
HRESULT GetILToNativeMapping(  
    [in] FunctionID functionId,  
    [in] ReJITID reJitId,  
    [in] ULONG32 cMap,  
    [out] ULONG32 *pcMap,  
    [out, size_is(cMap), length_is(*pcMap)]  
        COR_DEBUG_IL_TO_NATIVE_MAP map[]);  
```  
  
## <a name="parameters"></a>參數  
 `functionId`  
 [in] 包含程式碼的函式 ID。  
  
 `pReJitId`  
 [in] 經過 JIT 重新編譯的函式識別。 .NET Framework 4.5 中的身分識別必須為零。  
  
 `cMap`  
 [in] `map` 陣列的大小上限。  
  
 `pcMap`  
 脫銷可用的 COR_DEBUG_IL_TO_NATIVE_MAP 結構總數。  
  
 `map`  
 [out] `COR_DEBUG_IL_TO_NATIVE_MAP` 結構的陣列，每個結構都有指定位移。 `GetILToNativeMapping2` 方法傳回之後，`map` 將會包含部分或所有 `COR_DEBUG_IL_TO_NATIVE_MAP` 結構。  
  
## <a name="remarks"></a>備註  
 `GetILToNativeMapping2`類似于[ICorProfilerInfo:: GetILToNativeMapping](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo-getiltonativemapping-method.md)方法, 不同之處在于它可讓分析工具在未來的版本中指定重新編譯函式的識別碼。  
  
> [!NOTE]
> [ICorProfilerFunctionControl:: SetILInstrumentedCodeMap](../../../../docs/framework/unmanaged-api/profiling/icorprofilerfunctioncontrol-setilinstrumentedcodemap-method.md)方法不會在 .NET Framework 4.5 中執行, 因此已進行 JIT 重新編譯的函式不能有與原始編譯函式不同的 IL 與原生對應。 因此, 在`GetILToNativeMapping2` .NET Framework 4.5 中, 無法以非零的 JIT 重新編譯識別碼來呼叫。  
  
 `GetILToNativeMapping2` 方法會傳回 `COR_DEBUG_IL_TO_NATIVE_MAP` 結構的陣列。 為了傳達原生指令的特定範圍對應至程式碼的特殊區域 (例如初構), 陣列中的專案可以將其`ilOffset`欄位設定為[CorDebugIlToNativeMappingTypes](../../../../docs/framework/unmanaged-api/debugging/cordebugiltonativemappingtypes-enumeration.md)列舉的值。  
  
 `GetILToNativeMapping2` 傳回之後，您必須確認 `map` 緩衝區夠大，可以包含所有 `COR_DEBUG_IL_TO_NATIVE_MAP` 結構。 若要這樣做，請比較 `cMap` 的值與 `pcMap` 參數的值。 如果 `pcMap` 值乘以 `COR_DEBUG_IL_TO_NATIVE_MAP` 結構的大小之後大於 `cMap`，請配置較大的 `map` 緩衝區，以新的較大大小更新 `cMap`，然後重新呼叫 `GetILToNativeMapping2`。  
  
 或者，您也可以先使用長度為零的 `map` 緩衝區來呼叫 `GetILToNativeMapping2`，以取得正確的緩衝區大小。 接著您就可以將緩衝區大小設定為 `pcMap` 中傳回的值，並再次呼叫 `GetILToNativeMapping2`。  
  
## <a name="requirements"></a>需求  
 **平台：** 請參閱[系統需求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **標頭：** Corprof.idl .idl, Corprof.idl。h  
  
 **LIBRARY:** CorGuids.lib  
  
 **.NET framework 版本：** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另請參閱

- [GetILToNativeMapping 方法](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo-getiltonativemapping-method.md)
- [ICorProfilerInfo4 介面](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo4-interface.md)
- [分析介面](../../../../docs/framework/unmanaged-api/profiling/profiling-interfaces.md)
- [程式碼剖析](../../../../docs/framework/unmanaged-api/profiling/index.md)
