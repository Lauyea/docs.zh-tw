---
title: IXCLRDataProcess 介面
ms.date: 01/16/2019
api.name:
- IXCLRDataProcess Interface
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataProcess Interface
helpviewer.keywords:
- IXCLRDataProcess Interface [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 8c98dd42b4ac5593d96478c2286f49ad216a4bc1
ms.sourcegitcommit: b56d59ad42140d277f2acbd003b74d655fdbc9f1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2019
ms.locfileid: "54416433"
---
# <a name="ixclrdataprocess-interface"></a><span data-ttu-id="ed916-102">IXCLRDataProcess 介面</span><span class="sxs-lookup"><span data-stu-id="ed916-102">IXCLRDataProcess Interface</span></span>

<span data-ttu-id="ed916-103">提供方法來查詢處理序的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="ed916-103">Provides methods for querying information about a process.</span></span>

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="methods"></a><span data-ttu-id="ed916-104">方法</span><span class="sxs-lookup"><span data-stu-id="ed916-104">Methods</span></span>

| <span data-ttu-id="ed916-105">方法</span><span class="sxs-lookup"><span data-stu-id="ed916-105">Method</span></span>                                                                                                                                               | <span data-ttu-id="ed916-106">描述</span><span class="sxs-lookup"><span data-stu-id="ed916-106">Description</span></span>                                                                                     |
| ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| [<span data-ttu-id="ed916-107">GetAppDomainByUniqueId</span><span class="sxs-lookup"><span data-stu-id="ed916-107">GetAppDomainByUniqueId</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-getappdomainbyuniqueid-method.md)                       | <span data-ttu-id="ed916-108">取得`AppDomain`依其唯一識別碼的處理序中。</span><span class="sxs-lookup"><span data-stu-id="ed916-108">Gets an `AppDomain` in a process by its unique id.</span></span>                                              |
| [<span data-ttu-id="ed916-109">StartEnumModules</span><span class="sxs-lookup"><span data-stu-id="ed916-109">StartEnumModules</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-startenummodules-method.md)                                   | <span data-ttu-id="ed916-110">提供列舉的處理程序的模組控制代碼。</span><span class="sxs-lookup"><span data-stu-id="ed916-110">Provides a handle to enumerate the modules of a process.</span></span>                                        |
| [<span data-ttu-id="ed916-111">EnumModule</span><span class="sxs-lookup"><span data-stu-id="ed916-111">EnumModule</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-enummodule-method.md)                                               | <span data-ttu-id="ed916-112">列舉此程序的模組。</span><span class="sxs-lookup"><span data-stu-id="ed916-112">Enumerates the modules of this process.</span></span>                                                         |
| [<span data-ttu-id="ed916-113">EndEnumModules</span><span class="sxs-lookup"><span data-stu-id="ed916-113">EndEnumModules</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-endenummodules-method.md)                                       | <span data-ttu-id="ed916-114">釋出內部模組列舉期間使用的迭代器所使用的資源。</span><span class="sxs-lookup"><span data-stu-id="ed916-114">Releases the resources used by internal iterators used during module enumeration.</span></span>               |
| [<span data-ttu-id="ed916-115">StartEnumMethodInstancesByAddress</span><span class="sxs-lookup"><span data-stu-id="ed916-115">StartEnumMethodInstancesByAddress</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-startenummethodinstancesbyaddress-method.md) | <span data-ttu-id="ed916-116">提供列舉的方法執行個體的控制代碼`AppDomain`指定位址開頭。</span><span class="sxs-lookup"><span data-stu-id="ed916-116">Provides a handle to enumerate the method instances of `AppDomain` starting at a given address.</span></span> |
| [<span data-ttu-id="ed916-117">EnumMethodInstanceByAddress</span><span class="sxs-lookup"><span data-stu-id="ed916-117">EnumMethodInstanceByAddress</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-enummethodinstancebyaddress-method.md)             | <span data-ttu-id="ed916-118">列舉位址位移處開始此程序的方法執行的個體。</span><span class="sxs-lookup"><span data-stu-id="ed916-118">Enumerates the method instances of this process starting at an address offset.</span></span>                  |
| [<span data-ttu-id="ed916-119">EndEnumMethodInstancesByAddress</span><span class="sxs-lookup"><span data-stu-id="ed916-119">EndEnumMethodInstancesByAddress</span></span>](../../../../docs/framework/unmanaged-api/debugging/ixclrdataprocess-endenummethodinstancesbyaddress-method.md)     | <span data-ttu-id="ed916-120">釋放執行個體列舉期間使用的內部迭代器所使用的資源。</span><span class="sxs-lookup"><span data-stu-id="ed916-120">Releases the resources used by internal iterators used during instance enumeration.</span></span>             |

## <a name="remarks"></a><span data-ttu-id="ed916-121">備註</span><span class="sxs-lookup"><span data-stu-id="ed916-121">Remarks</span></span>

<span data-ttu-id="ed916-122">此介面的執行階段內，而且不會公開透過任何標頭或程式庫檔案。</span><span class="sxs-lookup"><span data-stu-id="ed916-122">This interface lives inside the runtime and is not exposed through any headers or library files.</span></span> <span data-ttu-id="ed916-123">不過，它是 COM 介面衍生自`IUnknown`含有 GUID `5c552ab6-fc09-4cb3-8e36-22fa03c798b7` ，可以透過一般的 COM 機制取得。</span><span class="sxs-lookup"><span data-stu-id="ed916-123">However, it's a COM interface that derives from `IUnknown` with GUID `5c552ab6-fc09-4cb3-8e36-22fa03c798b7` that can be obtained through the usual COM mechanisms.</span></span>

## <a name="requirements"></a><span data-ttu-id="ed916-124">需求</span><span class="sxs-lookup"><span data-stu-id="ed916-124">Requirements</span></span>

<span data-ttu-id="ed916-125">**平台：** 請參閱[系統需求](../../../../docs/framework/get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="ed916-125">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>   
<span data-ttu-id="ed916-126">**標頭：** 無</span><span class="sxs-lookup"><span data-stu-id="ed916-126">**Header:** None</span></span>  
<span data-ttu-id="ed916-127">**程式庫：** 無</span><span class="sxs-lookup"><span data-stu-id="ed916-127">**Library:** None</span></span>  
<span data-ttu-id="ed916-128">**.NET framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span><span class="sxs-lookup"><span data-stu-id="ed916-128">**.NET Framework Versions:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span></span>  

## <a name="see-also"></a><span data-ttu-id="ed916-129">另請參閱</span><span class="sxs-lookup"><span data-stu-id="ed916-129">See Also</span></span>

- [<span data-ttu-id="ed916-130">偵錯</span><span class="sxs-lookup"><span data-stu-id="ed916-130">Debugging</span></span>](../../../../docs/framework/unmanaged-api/debugging/index.md)
- [<span data-ttu-id="ed916-131">偵錯介面</span><span class="sxs-lookup"><span data-stu-id="ed916-131">Debugging Interfaces</span></span>](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)