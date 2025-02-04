---
title: 比較語意 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: b36ce28a-2fe4-4236-b782-e5f7c054deae
ms.openlocfilehash: da7b8f662d10376abd649e674701b43b7b740a6f
ms.sourcegitcommit: 4e2d355baba82814fa53efd6b8bbb45bfe054d11
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/04/2019
ms.locfileid: "70251186"
---
# <a name="comparison-semantics-entity-sql"></a>比較語意 (Entity SQL)
執行以下任何 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 運算子都會牽涉到型別執行個體的比較：  
  
## <a name="explicit-comparison"></a>明確比較  
 相等運算：  
  
- =  
  
- !=  
  
 排序作業：  
  
- <  
  
- \<=  
  
- \>  
  
- \>=  
  
 可為 Null 的運算：  
  
- IS NULL  
  
- IS NOT NULL  
  
## <a name="explicit-distinction"></a>明確區分  
 相等區分：  
  
- DISTINCT  
  
- GROUP BY  
  
 排序區分：  
  
- ORDER BY  
  
## <a name="implicit-distinction"></a>隱含區分  
 Set 作業和述詞 (相等)：  
  
- UNION  
  
- INTERSECT  
  
- EXCEPT  
  
- SET  
  
- OVERLAPS  
  
 Item 述詞 (相等)：  
  
- IN  
  
## <a name="supported-combinations"></a>支援的組合  
 以下資料表針對每一種型別顯示比較運算子的所有支援組合：  
  
|**型別**|**=**<br /><br /> **!=**|**GROUP BY**<br /><br /> **DISTINCT**|**UNION**<br /><br /> **INTERSECT**<br /><br /> **EXCEPT**<br /><br /> **SET**<br /><br /> **OVERLAPS**|**IN**|**<   <=**<br /><br /> **>   >=**|**ORDER BY**|**為 NULL**<br /><br /> **不是 NULL**|  
|-|-|-|-|-|-|-|-|  
|實體類型|Ref<sup>1</sup>|所有屬性<sup>2</sup>|所有屬性<sup>2</sup>|所有屬性<sup>2</sup>|擲回<sup>3</sup>|擲回<sup>3</sup>|Ref<sup>1</sup>|  
|複雜類型|擲回<sup>3</sup>|擲回<sup>3</sup>|擲回<sup>3</sup>|擲回<sup>3</sup>|擲回<sup>3</sup>|擲回<sup>3</sup>|擲回<sup>3</sup>|  
|表格列|所有屬性<sup>4</sup>|所有屬性<sup>4</sup>|所有屬性<sup>4</sup>|擲回<sup>3</sup>|擲回<sup>3</sup>|所有屬性<sup>4</sup>|擲回<sup>3</sup>|  
|基本型別|提供者特定|提供者特定|提供者特定|提供者特定|提供者特定|提供者特定|提供者特定|  
|多重集|擲回<sup>3</sup>|擲回<sup>3</sup>|擲回<sup>3</sup>|擲回<sup>3</sup>|擲回<sup>3</sup>|擲回<sup>3</sup>|擲回<sup>3</sup>|  
|參考|是<sup>5</sup>|是<sup>5</sup>|是<sup>5</sup>|是<sup>5</sup>|Throw|Throw|是<sup>5</sup>|  
|關聯<br /><br /> 型別|擲回<sup>3</sup>|Throw|Throw|Throw|擲回<sup>3</sup>|擲回<sup>3</sup>|擲回<sup>3</sup>|  
  
 <sup>1</sup>指定之實體類型實例的參考會以隱含方式進行比較, 如下列範例所示:  
  
```  
SELECT p1, p2   
FROM AdventureWorksEntities.Product AS p1   
     JOIN AdventureWorksEntities.Product AS p2   
WHERE p1 != p2 OR p1 IS NULL  
```  
  
 實體執行個體無法與明確參考相比較。 如果嘗試進行此作業，就會擲回例外狀況。 例如，下列查詢將會擲回例外狀況：  
  
```  
SELECT p1, p2   
FROM AdventureWorksEntities.Product AS p1   
     JOIN AdventureWorksEntities.Product AS p2   
WHERE p1 != REF(p2)  
```  
  
 <sup>2</sup>複雜類型的屬性會在傳送至存放區之前先簡維, 使其成為可比較的 (只要其所有屬性都是可比較的)。 另請參閱<sup>4。</sup>  
  
 <sup>3</sup>Entity Framework 執行時間會偵測到不支援的情況, 並擲回有意義的例外狀況, 而不需要參與提供者/存放  
  
 <sup>4</sup>嘗試比較所有屬性。 如果某個屬性具有無法比較的型別 (如文字、ntext 或影像)，可能會擲回伺服器例外狀況。  
  
 <sup>5</sup>會比較參考的所有個別元素 (這包括實體集名稱和實體類型的所有索引鍵屬性)。  
  
## <a name="see-also"></a>另請參閱

- [Entity SQL 概觀](entity-sql-overview.md)
