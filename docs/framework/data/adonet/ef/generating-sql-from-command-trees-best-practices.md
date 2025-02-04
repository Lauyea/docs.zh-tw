---
title: 從命令樹產生 SQL - 最佳作法
ms.date: 03/30/2017
ms.assetid: 71ef6a24-4c4f-4254-af3a-ffc0d855b0a8
ms.openlocfilehash: 9859c7df941ae6681c991001e0d1e5a50c7ffc60
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70855010"
---
# <a name="generating-sql-from-command-trees---best-practices"></a>從命令樹產生 SQL - 最佳作法

輸出查詢命令樹會仔細地建立可用 SQL 表示之查詢的模型。 不過，從輸出命令樹產生 SQL 時，提供者寫入器會面臨某些常見的挑戰。 本主題將討論這些挑戰。 在下一個主題中，範例提供者將示範如何處理這些挑戰。

## <a name="group-dbexpression-nodes-in-a-sql-select-statement"></a>在 SQL SELECT 陳述式中分組 DbExpression 節點

一般 SQL 陳述式具有下列形狀的巢狀結構：

```sql
SELECT …
FROM …
WHERE …
GROUP BY …
ORDER BY …
```

一個或多個子句可能是空的。  巢狀 SELECT 陳述式可能會出現於任何一行。

將查詢命令樹轉譯成 SQL SELECT 陳述式的可能轉譯方式會針對每個關係運算子產生一個子查詢。 不過，這種結果可能會導致難以讀取的非必要巢狀子查詢。  在某些資料存放區上，查詢的執行效能可能會不佳。

例如，請考量下列查詢命令樹：

```
Project (
a.x,
   a = Filter(
      b.y = 5,
      b = Scan("TableA")
   )
)
```

沒有效率的轉譯會產生：

```sql
SELECT a.x
FROM (   SELECT *
         FROM TableA as b
         WHERE b.y = 5) as a
```

請注意，每個關聯運算式節點都會成為新的 SQL SELECT 陳述式。

因此，請務必盡可能將運算式節點彙總成單一 SQL SELECT 陳述式，同時保留正確性。

上述範例的這種彙總結果為：

```sql
SELECT b.x
FROM TableA as b
WHERE b.y = 5
```

## <a name="flatten-joins-in-a-sql-select-statement"></a>在 SQL SELECT 陳述式中扁平化聯結

將多個節點彙總成單一 SQL SELECT 陳述式的其中一個案例就是將多個聯結運算式彙總成單一 SQL SELECT 陳述式。 DbJoinExpression 代表介於兩個輸入之間的單一聯結。 不過，您可以將多個聯結指定為單一 SQL SELECT 陳述式的一部分。 在該案例中，這些聯結會依照指定的順序執行。

您可以更輕鬆地將左背面聯結 (顯示為另一個聯結之左子系的聯結) 扁平化為單一 SQL SELECT 陳述式。 例如，請考量下列查詢命令樹：

```
InnerJoin(
   a = LeftOuterJoin(
   b = Extent("TableA")
   c = Extent("TableB")
   ON b.y = c.x ),
   d = Extent("TableC")
   ON a.b.y = d.z
)
```

這個命令樹可正確轉譯成：

```sql
SELECT *
FROM TableA as b
LEFT OUTER JOIN TableB as c ON b.y = c.x
INNER JOIN TableC as d ON b.y = d.z
```

不過，您無法輕鬆地扁平化非左背面聯結，而且不應該嘗試扁平化這些聯結。 例如，下列查詢命令樹中的聯結：

```
InnerJoin(
   a = Extent("TableA")
   b = LeftOuterJoin(
   c = Extent("TableB")
   d = Extent("TableC")
   ON c.y = d.x),
   ON a.z = b.c.y
)
```

會轉譯成具有子查詢的 SQL SELECT 陳述式。

```sql
SELECT *
FROM TableA as a
INNER JOIN (SELECT *
   FROM TableB as c
   LEFT OUTER JOIN TableC as d
   ON c.y = d.x) as b
ON b.y = d.z
```

## <a name="input-alias-redirecting"></a>輸入別名重新導向

為了說明輸入別名重新導向，請考量關聯運算式的結構，例如 DbFilterExpression、DbProjectExpression、DbCrossJoinExpression、DbJoinExpression、DbSortExpression、DbGroupByExpression、DbApplyExpression 和 DbSkipExpression。

其中每一個型別都具有一個或多個描述輸入集合的 Input 屬性，而且對應至每個輸入的繫結變數會在集合周遊期間用來代表該輸入的每個項目。 例如，參考 DbFilterExpression 之 Predicate 屬性或 DbProjectExpression 之 Projection 屬性中的輸入項目時，就會使用此繫結程序變數。

將多個關聯運算式節點彙總成單一 SQL SELECT 陳述式，並且評估屬於關聯運算式一部分的運算式 (例如，屬於 DbProjectExpression 之 Projection 屬性的一部分) 時，它所使用的繫結變數可能不會與輸入的別名相同，因為多個運算式繫結必須重新導向至單一範圍。  這個問題就稱為別名重新命名。

請考量本主題的第一個範例。 如果要進行自然轉譯並且轉譯 Projection a.x (DbPropertyExpression(a, x))，將它轉譯成 `a.x` 是正確的作法，因為我們已經將輸入的別名指定為 "a"，以符合繫結變數。  不過，將這兩個節點彙總成單一 SQL SELECT 陳述式時，您就必須將相同的 DbPropertyExpression 轉譯成 `b.x`，因為輸入已經具有 "b" 的別名。

## <a name="join-alias-flattening"></a>聯結別名扁平化

與輸出命令樹中任何其他關聯運算式不同的是，DbJoinExpression 所輸出的結果型別是包含兩個資料行的資料列，而且其中每一個資料行都會對應至其中一個輸入。 建立 DbPropertyExpression 以存取源自于聯結的純量屬性時，它會超過另一個 DbPropertyExpression。

範例包括範例 2 中的 "a.b.y" 以及範例 3 中的 "b.c.y"。 不過，在對應的 SQL 陳述式中，這些項目都會當做 "b.y" 參考。 這種重新指定別名的作業稱為聯結別名扁平化。

## <a name="column-name-and-extent-alias-renaming"></a>資料行名稱和範圍別名重新命名

如果某個具有聯結的 SQL SELECT 查詢必須透過投影完成，從輸入列舉所有參與的資料行時，可能會發生名稱衝突，因為多個輸入可能具有相同的資料行名稱。 若要避免衝突，請針對資料行使用不同的名稱。

此外，在扁平化聯結時，參與的資料表 (或子查詢) 可能會具有衝突的別名，因此必須重新命名這些別名。

## <a name="avoid-select-"></a>避免使用 SELECT *

請勿使用 `SELECT *`，從基底資料表中選取。 Entity Framework 應用程式中的儲存體模型只能包含資料庫資料表中的資料行子集。 在此情況下，`SELECT *` 可能會產生錯誤的結果。 因此，您應該改用參與運算式之結果型別中的資料行名稱來指定所有參與的資料行。

## <a name="reuse-of-expressions"></a>重複使用運算式

運算式可以在 Entity Framework 所傳遞的查詢命令樹中重複使用。 請勿假設每個運算式都只會出現在查詢命令樹中一次。

## <a name="mapping-primitive-types"></a>對應基本型別

將概念 (EDM) 型別對應至提供者型別時，您應該對應至最廣泛的型別 (Int32)，以便容納所有可能的值。 此外，請避免對應至無法用於許多作業的型別，例如 BLOB 型別（ `ntext`如 SQL Server 中的）。

## <a name="see-also"></a>另請參閱

- [SQL 產生](sql-generation.md)
