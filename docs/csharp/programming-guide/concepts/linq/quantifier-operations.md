---
title: 數量詞作業 (C#)
ms.date: 07/20/2015
ms.assetid: 84ac2ac2-7a63-4581-bc4c-14e34be1493b
ms.openlocfilehash: 4a0f5b2c90d4b71a945dee02a32cbe897818c538
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2019
ms.locfileid: "69591473"
---
# <a name="quantifier-operations-c"></a>數量詞作業 (C#)
數量詞作業會傳回 <xref:System.Boolean> 值，指出序列中的部分或所有項目是否符合條件。  
  
 下圖說明兩個不同來源序列上的兩個不同數量詞作業。 第一個作業會詢問一個或多個項目是否為字元 'A'，而結果為 `true`。 第二個作業會詢問所有項目是否都為字元 'A'，而結果為 `true`。  
  
 ![LINQ 數量詞作業](./media/quantifier-operations/linq-quantifier-operations.png)  
  
 下節列出執行數量詞作業的標準查詢運算子方法。  
  
## <a name="methods"></a>方法  
  
|方法名稱|說明|C# 查詢運算式語法|更多資訊|  
|-----------------|-----------------|---------------------------------|----------------------|  
|All|判斷序列中的所有項目是否都符合條件。|不適用。|<xref:System.Linq.Enumerable.All%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.All%2A?displayProperty=nameWithType>|  
|Any|判斷序列中的任何項目是否符合條件。|不適用。|<xref:System.Linq.Enumerable.Any%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Any%2A?displayProperty=nameWithType>|  
|Contains|判斷序列是否包含指定的項目。|不適用。|<xref:System.Linq.Enumerable.Contains%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Contains%2A?displayProperty=nameWithType>|  
  
## <a name="see-also"></a>另請參閱

- <xref:System.Linq>
- [標準查詢運算子概觀 (C#)](./standard-query-operators-overview.md)
- [如何：在執行階段動態指定述詞篩選](../../linq-query-expressions/how-to-dynamically-specify-predicate-filters-at-runtime.md)
- [如何：查詢包含指定字組的句子 (LINQ) (C#)](./how-to-query-for-sentences-that-contain-a-specified-set-of-words-linq.md)
