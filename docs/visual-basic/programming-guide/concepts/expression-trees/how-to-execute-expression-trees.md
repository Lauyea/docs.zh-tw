---
title: 作法：執行運算式樹狀架構 (Visual Basic)
ms.date: 07/20/2015
ms.assetid: 9dfb5ab3-f48f-417e-975f-f8f6f1cdc18d
ms.openlocfilehash: 135c295070ea591f3b494734f9d236e36b9c3c5d
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2019
ms.locfileid: "69916507"
---
# <a name="how-to-execute-expression-trees-visual-basic"></a>作法：執行運算式樹狀架構 (Visual Basic)
本主題示範如何執行運算式樹狀架構。 執行運算式樹狀架構可能會傳回一個值，或者只是執行某個動作，例如呼叫方法。  
  
 您只能執行代表 Lambda 運算式的運算式樹狀架構。 代表 Lambda 運算式的運算式樹狀架構為 <xref:System.Linq.Expressions.LambdaExpression> 或 <xref:System.Linq.Expressions.Expression%601> 類型。 若要執行這些運算式樹狀架構，請呼叫 <xref:System.Linq.Expressions.LambdaExpression.Compile%2A> 方法建立可執行委派，然後叫用該委派。  
  
> [!NOTE]
> 如果不知道委派的類型，也就是 lambda 運算式是類型 <xref:System.Linq.Expressions.LambdaExpression> 而非 <xref:System.Linq.Expressions.Expression%601>，您必須對委派呼叫 <xref:System.Delegate.DynamicInvoke%2A> 方法，而不是直接叫用它。  
  
 如果運算式樹狀架構不代表 Lambda 運算式，您可以呼叫 <xref:System.Linq.Expressions.Expression.Lambda%60%601%28System.Linq.Expressions.Expression%2CSystem.Collections.Generic.IEnumerable%7BSystem.Linq.Expressions.ParameterExpression%7D%29> 方法，建立新的 Lambda 運算式，以原始的運算式樹狀架構當做其主體。 然後，您可以如本節稍早所述來執行此 Lambda 運算式。  
  
## <a name="example"></a>範例  
 下列程式碼範例示範如何藉由建立和執行 Lambda 運算式，來執行代表數字自乘至乘冪的運算式樹狀架構。 顯示的結果會是已自乘至乘冪的數字。  
  
```vb  
' The expression tree to execute.  
Dim be As BinaryExpression = Expression.Power(Expression.Constant(2.0R), Expression.Constant(3.0R))  
  
' Create a lambda expression.  
Dim le As Expression(Of Func(Of Double)) = Expression.Lambda(Of Func(Of Double))(be)  
  
' Compile the lambda expression.  
Dim compiledExpression As Func(Of Double) = le.Compile()  
  
' Execute the lambda expression.  
Dim result As Double = compiledExpression()  
  
' Display the result.  
MsgBox(result)  
  
' This code produces the following output:  
' 8  
```  
  
## <a name="compiling-the-code"></a>編譯程式碼  
  
- 加入 System.Linq.Expressions 命名空間。  
  
## <a name="see-also"></a>另請參閱

- [運算式樹狀結構 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/expression-trees/index.md)
- [如何：修改運算式樹狀架構 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/expression-trees/how-to-modify-expression-trees.md)
