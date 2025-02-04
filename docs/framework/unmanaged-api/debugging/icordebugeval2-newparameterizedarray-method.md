---
title: ICorDebugEval2::NewParameterizedArray 方法
ms.date: 03/30/2017
api_name:
- ICorDebugEval2.NewParameterizedArray
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugEval2::NewParameterizedArray
helpviewer_keywords:
- ICorDebugEval2::NewParameterizedArray method [.NET Framework debugging]
- NewParameterizedArray method [.NET Framework debugging]
ms.assetid: 45efb8ba-c4de-4109-945f-e734d376b43c
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 973f975885bbbf5cbed74adef7b9f4f423c42583
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67753662"
---
# <a name="icordebugeval2newparameterizedarray-method"></a>ICorDebugEval2::NewParameterizedArray 方法
配置的指定項目類型和維度的新陣列。  
  
## <a name="syntax"></a>語法  
  
```cpp  
HRESULT NewParameterizedArray(  
    [in] ICorDebugType          *pElementType,  
    [in] ULONG32                rank,  
    [in, size_is(rank)] ULONG32 dims[],  
    [in, size_is(rank)] ULONG32 lowBounds[]  
);  
```  
  
## <a name="parameters"></a>參數  
 `pElementType`  
 [in]ICorDebugType 物件，表示儲存在陣列中元素的類型指標。  
  
 `rank`  
 [in]陣列維度的數目。 在.NET Framework 2.0 版中，此值必須是 1。  
  
 `dims`  
 [in]以位元組為單位，每個陣列維度大小。  
  
 `lowBounds`  
 [in] 選用。 陣列的每個維度的下限。 如果省略此值，則會假設每個維度下限為零。  
  
## <a name="remarks"></a>備註  
 陣列的項目可以是泛型類型的執行個體。 這個陣列是一律會建立目前執行中執行緒的應用程式定義域中。 在.NET Framework 2.0 的值`rank`必須是 1。  
  
## <a name="requirements"></a>需求  
 **平台：** 請參閱[系統需求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **標頭：** CorDebug.idl、CorDebug.h  
  
 **LIBRARY:** CorGuids.lib  
  
 **.NET framework 版本：** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
