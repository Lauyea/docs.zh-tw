---
title: CREATEREF (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 489828cf-a335-4449-9360-b0d92eec5481
ms.openlocfilehash: cbaea82108dd3debcca972ca15dea248227330ac
ms.sourcegitcommit: 4e2d355baba82814fa53efd6b8bbb45bfe054d11
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/04/2019
ms.locfileid: "70251108"
---
# <a name="createref-entity-sql"></a>CREATEREF (Entity SQL)
建立實體集中實體的參考。  
  
## <a name="syntax"></a>語法  
  
```  
CreateRef(entityset_identifier, row_typed_expression)  
```  
  
## <a name="arguments"></a>引數  
 `entityset_identifier`  
 實體集識別項，非字串常值。  
  
 `row_typed_expression`  
 對應到實體類型的索引鍵屬性的資料列型別運算式。  
  
## <a name="remarks"></a>備註  
 `row_typed_expression` 在結構上必須相當於實體的索引鍵型別。 換言之，它必須擁有與實體索引鍵相同的欄位數目和型別，而且順序相同。  
  
 在下面的範例中，Orders 和 BadOrders 兩者都是 Order 型別的實體集，而 Id 則相當於 Order 的單一索引鍵屬性。 此範例說明如何產生 BadOrders 中實體的產考。 請留意，此參考可能會懸空。  換言之，此參考可能無法實際識別特定的實體。 在這種情況下，對該參考的 `DEREF` 作業將會傳回 null。  
  
```  
select CreateRef(LOB.BadOrders, row(o.Id))   
from LOB.Orders as o   
```  
  
## <a name="example"></a>範例  
 下列 Entity SQL 查詢使用 CREATEREF 運算子來建立實體集中實體的參考。 此查詢是根據 AdventureWorks Sales Model。 若要編譯及執行此查詢，請遵循以下步驟：  
  
1. [遵循 how to：執行可傳回 StructuralType 結果](../how-to-execute-a-query-that-returns-structuraltype-results.md)的查詢。  
  
2. 將下列查詢當成引數，傳遞至 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-csharp[DP EntityServices Concepts 2#CREATEREF](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts 2/cs/entitysql.cs#createref)]  
  
## <a name="see-also"></a>另請參閱

- [Entity SQL 參考](entity-sql-reference.md)
- [DEREF](deref-entity-sql.md)
- [KEY](key-entity-sql.md)
- [REF](ref-entity-sql.md)
