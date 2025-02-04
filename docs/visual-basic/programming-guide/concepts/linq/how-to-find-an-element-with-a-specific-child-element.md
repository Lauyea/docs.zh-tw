---
title: HOW TO：尋找具有特定子項目的元素 (Visual Basic)
ms.date: 07/20/2015
ms.assetid: b0d0a463-6a85-46c3-8453-ad25b0ecf93c
ms.openlocfilehash: af8667b6aa6870accb62fa22bd5243ce029b32c9
ms.sourcegitcommit: eb9ff6f364cde6f11322e03800d8f5ce302f3c73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/01/2019
ms.locfileid: "68709061"
---
# <a name="how-to-find-an-element-with-a-specific-child-element-visual-basic"></a>作法：尋找具有特定子項目的元素 (Visual Basic)
本主題顯示如何利用特定的值尋找具有子項目的特定項目。  
  
## <a name="example"></a>範例  
 此範例會利用 "Examp2.EXE" 這個值，尋找具有 `Test` 子項目的 `CommandLine` 項目。  
  
 此範例使用下列 XML 文件：[XML 範例檔：測試組態 (LINQ to XML)](../../../../visual-basic/programming-guide/concepts/linq/sample-xml-file-test-configuration-linq-to-xml.md)。  
  
```vb  
Dim root As XElement = XElement.Load("TestConfig.xml")  
Dim tests As IEnumerable(Of XElement) = _  
    From el In root.<Test> _  
    Where el.<CommandLine>.Value = "Examp2.EXE" _  
    Select el  
For Each el as XElement In tests  
    Console.WriteLine(el.@TestId)  
Next  
```  
  
 此程式碼會產生下列輸出：  
  
```  
0002  
0006  
```  
  
 請注意, 此範例使用[Xml 子軸屬性](../../../../visual-basic/language-reference/xml-axis/xml-child-axis-property.md)、 [xml 屬性軸屬性](../../../../visual-basic/language-reference/xml-axis/xml-attribute-axis-property.md)和[xml 值屬性](../../../../visual-basic/language-reference/xml-axis/xml-value-property.md)。  
  
## <a name="example"></a>範例  
 下列範例顯示命名空間中之 XML 的相同查詢。 如需詳細資訊, 請參閱[命名空間總覽 (LINQ to XML) (Visual Basic)](namespaces-overview-linq-to-xml.md)。  
  
 此範例使用下列 XML 文件：[XML 範例檔：測試命名空間中的組態](../../../../visual-basic/programming-guide/concepts/linq/sample-xml-file-test-configuration-in-a-namespace.md)。  
  
```vb  
Imports <xmlns='http://www.adatum.com'>  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = XElement.Load("TestConfigInNamespace.xml")  
        Dim tests As IEnumerable(Of XElement) = _  
            From el In root.<Test> _  
            Where el.<CommandLine>.Value = "Examp2.EXE" _  
            Select el  
        For Each el As XElement In tests  
            Console.WriteLine(el.@TestId)  
        Next  
    End Sub  
End Module  
```  
  
 此程式碼會產生下列輸出：  
  
```  
0002  
0006  
```  
  
## <a name="see-also"></a>另請參閱

- <xref:System.Xml.Linq.XElement.Attribute%2A>
- <xref:System.Xml.Linq.XContainer.Elements%2A>
- [基本查詢 (LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/basic-queries-linq-to-xml.md)
- [標準查詢運算子概觀 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/standard-query-operators-overview.md)
- [投射作業 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/projection-operations.md)
