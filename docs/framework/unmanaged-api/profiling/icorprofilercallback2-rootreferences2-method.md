---
title: ICorProfilerCallback2::RootReferences2 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback2.RootReferences2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback2::RootReferences2
helpviewer_keywords:
- RootReferences2 method [.NET Framework profiling]
- ICorProfilerCallback2::RootReferences2 method [.NET Framework profiling]
ms.assetid: 55a2f907-d216-42eb-8f2f-e5d59c2eebd6
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 563a2e19c9c254870b3e767253a276a201e631a6
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67779299"
---
# <a name="icorprofilercallback2rootreferences2-method"></a>ICorProfilerCallback2::RootReferences2 方法
進行記憶體回收發生後，請通知分析工具之根參考。 這個方法是擴充功能[icorprofilercallback:: Rootreferences](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-rootreferences-method.md)方法。  
  
## <a name="syntax"></a>語法  
  
```cpp  
HRESULT RootReferences2(  
    [in] ULONG  cRootRefs,  
    [in, size_is(cRootRefs)] ObjectID rootRefIds[],  
    [in, size_is(cRootRefs)] COR_PRF_GC_ROOT_KIND rootKinds[],  
    [in, size_is(cRootRefs)] COR_PRF_GC_ROOT_FLAGS rootFlags[],  
    [in, size_is(cRootRefs)] UINT_PTR rootIds[]);  
```  
  
## <a name="parameters"></a>參數  
 `cRootRefs`  
 [in]中的項目數`rootRefIds`， `rootKinds`， `rootFlags`，和`rootIds`陣列。  
  
 `rootRefIds`  
 [in]物件識別碼，其中每個參考的靜態物件或堆疊上的物件陣列。 中的項目`rootKinds`陣列提供要分類中的對應元素的資訊`rootRefIds`陣列。  
  
 `rootKinds`  
 [in]陣列[COR_PRF_GC_ROOT_KIND](../../../../docs/framework/unmanaged-api/profiling/cor-prf-gc-root-kind-enumeration.md)值，指出記憶體回收根目錄的類型。  
  
 `rootFlags`  
 [in]陣列[COR_PRF_GC_ROOT_FLAGS](../../../../docs/framework/unmanaged-api/profiling/cor-prf-gc-root-flags-enumeration.md)描述記憶體回收根目錄屬性的值。  
  
 `rootIds`  
 [in]值指向包含記憶體回收根目錄，根據的值的其他資訊的整數的陣列的 UINT_PTR`rootKinds`參數。  
  
 如果根型別是堆疊，根識別碼是包含變數的函式。 如果該根識別碼為 0，函數就會是內部，CLR 不具名函式。 如果根型別是控制代碼，根識別碼很適合記憶體回收控制代碼。 對於其他根型別，ID 會是不透明值，而且應該被忽略。  
  
## <a name="remarks"></a>備註  
 `rootRefIds`， `rootKinds`， `rootFlags`，和`rootIds`陣列是平行陣列。 亦即`rootRefIds[i]`， `rootKinds[i]`， `rootFlags[i]`，和`rootIds[i]`全都會考量相同的根。  
  
 兩者`RootReferences`和`RootReferences2`呼叫以通知分析工具。 程式碼剖析工具將通常實作其中一種方法或其他的但非兩者，因為傳入的資訊`RootReferences2`傳入的超集`RootReferences`。  
  
 中的項目可能`rootRefIds`成為零，表示對應的根目錄參考為 null，且不是指在 managed 堆積上的物件。  
  
 所傳回的物件識別碼`RootReferences2`不本身的回呼期間有效，因為記憶體回收可能正在將物件從舊位址移到新的位址。 因此，分析工具不應嘗試在 `RootReferences2` 呼叫期間檢查物件。 當[ICorProfilerCallback2::GarbageCollectionFinished](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback2-garbagecollectionfinished-method.md)是呼叫，所有物件都已移至其新位置和可以安全地進行檢查。  
  
## <a name="requirements"></a>需求  
 **平台：** 請參閱[系統需求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **標頭：** CorProf.idl, CorProf.h  
  
 **LIBRARY:** CorGuids.lib  
  
 **.NET framework 版本：** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另請參閱

- [ICorProfilerCallback 介面](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-interface.md)
- [ICorProfilerCallback2 介面](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback2-interface.md)
