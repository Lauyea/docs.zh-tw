---
title: + (加號)
ms.date: 03/30/2017
ms.assetid: 51769b02-a8f7-4177-9e99-bbd10e77092c
ms.openlocfilehash: 8c9a6b2c8168e4677c37cfdb0b401a93ee0040cf
ms.sourcegitcommit: 4e2d355baba82814fa53efd6b8bbb45bfe054d11
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/04/2019
ms.locfileid: "70251363"
---
# <a name="-add"></a>+ (加號)
將兩個數字相加。  
  
## <a name="syntax"></a>語法  
  
```  
expression + expression  
```  
  
## <a name="arguments"></a>引數  
 `expression`  
 任何一個數值資料型別的任何有效運算式。  
  
## <a name="result-types"></a>結果型別  
 從兩個引數的隱含型別提升產生的資料型別。 如需隱含類型升級的詳細資訊, 請參閱[類型系統](type-system-entity-sql.md)。  
  
## <a name="remarks"></a>備註  
 若為 EDM.String 型別，加法就會串連。  
  
## <a name="example"></a>範例  
 下列 Entity SQL 查詢會使用 + 算術運算子讓兩個數字相加。 此查詢是根據 AdventureWorks Sales Model。 若要編譯及執行此查詢，請遵循以下步驟：  
  
1. [遵循 how to:執行可傳回 StructuralType 結果](../how-to-execute-a-query-that-returns-structuraltype-results.md)的查詢。  
  
2. 將下列查詢當成引數，傳遞至 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-csharp[DP EntityServices Concepts 2#ADD](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts 2/cs/entitysql.cs#add)]  
  
## <a name="see-also"></a>另請參閱

- [Entity SQL 參考](entity-sql-reference.md)
- [概念模型類型 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#conceptual-model-types-csdl)
