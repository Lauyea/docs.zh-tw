---
title: 作法：從型別程式庫產生 Interop 組件
ms.date: 03/30/2017
helpviewer_keywords:
- importing type library
- interop assemblies, generating
- Type Library Importer
- type libraries
- COM interop, importing type library
ms.assetid: 4afd40c3-68f2-41c5-8ec1-4951bc148b9c
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: fcdff732afce90f725f4730f0054296e389ada1b
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2019
ms.locfileid: "71051792"
---
# <a name="how-to-generate-interop-assemblies-from-type-libraries"></a>作法：從型別程式庫產生 Interop 組件
[型別程式庫匯入工具 (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md) 是一種命令列工具，可將 COM 類型程式庫中包含的 Coclass 和介面轉換為中繼資料。 這個工具會自動為類型資訊建立 Interop 組件和命名空間。 當類別的中繼資料可供使用之後，Managed 用戶端就可以建立 COM 類型的執行個體並呼叫其方法，就如同它是 .NET 執行個體一樣。 Tlbimp.exe 會一次將整個型別程式庫轉換為中繼資料，而且不能為型別程式庫中定義的類型子集產生類型資訊。  
  
### <a name="to-generate-an-interop-assembly-from-a-type-library"></a>從型別程式庫產生 Interop 組件  
  
1. 請使用以下命令：  
  
     **tlbimp** \<*type-library-file*>  
  
     新增 **/out:** 參數會產生已變更名稱的 Interop 組件，例如 LOANLib.dll。 變更 Interop 組件的名稱有助於與原始 COM DLL 區別，且可避免因為具有重複名稱而產生的問題。  
  
## <a name="example"></a>範例  
 下列命令會在 `Loanlib` 命名空間中產生 Loanlib.dll 組件。  
  
```  
tlbimp Loanlib.tlb  
```  
  
 下列命令會產生已變更名稱 (LOANLib.dll) 的 Interop 組件。  
  
```  
tlbimp LoanLib.tlb /out: LOANLib.dll  
```  
  
## <a name="see-also"></a>另請參閱

- [匯入類型程式庫做為組件](importing-a-type-library-as-an-assembly.md)
- [將 COM 元件公開給 .NET Framework](exposing-com-components.md)
