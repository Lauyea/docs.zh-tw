---
title: HOW TO：了解傳遞結構和傳遞類別參考給方法之間的差異 - C# 程式設計手冊
ms.custom: seodec18
ms.date: 07/20/2015
helpviewer_keywords:
- structs [C#], passing as method parameter
- passing parameters [C#], structs vs. classes
- methods [C#], passing classes vs. structs
ms.assetid: 9c1313a6-32a8-4ea7-a59f-450f66af628b
ms.openlocfilehash: 09b40a1ee8ab57a4b8a641acae49ab31a14d3d5b
ms.sourcegitcommit: 5ae5a1a9520b8b8b6164ad728d396717f30edafc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/11/2019
ms.locfileid: "70893148"
---
# <a name="how-to-know-the-difference-between-passing-a-struct-and-passing-a-class-reference-to-a-method-c-programming-guide"></a>HOW TO：了解傳遞結構和傳遞類別參考給方法之間的差異 (C# 程式設計手冊)
下例示範將 [struct](../../language-reference/keywords/struct.md) 傳遞給方法，與將 [class](../../language-reference/keywords/class.md) 執行個體傳遞給方法有何不同。 在此範例中，兩個引數 (struct 和 class 執行個體) 都是以傳值方式傳遞，而且兩種方法都會變更引數其中一個欄位的值。 不過，兩個方法的結果不相同，因為傳遞 struct 時所傳送內容，和傳遞 class 執行個體時所傳送的內容不同。  
  
 因為 struct 是[實值型別](../../language-reference/keywords/value-types.md)，所以當您[以傳值方式傳遞 struct](./passing-value-type-parameters.md) 給方法時，方法會收到 struct 引數的複本並在其上運作。 方法無法存取呼叫方法中的原始 struct，因此無法以任何方式變更它。 方法只能變更複本。  
  
 class 執行個體是[參考型別](../../language-reference/keywords/reference-types.md)，不是實值型別。 當[以傳值方式傳遞參考型別](./passing-reference-type-parameters.md)給方法時，方法會收到 class 執行個體的參考複本。 也就是說，被呼叫的方法會接收實例的位址複本，而呼叫的方法會保留實例的原始位址。 呼叫方法中的 class 執行個體有位址，被呼叫方法中的參數有位址複本，而且這兩個位址都指向相同的物件。 因為參數只包含位址的複本，所以被呼叫的方法無法變更呼叫方法中的 class 執行個體的位址。 不過，被呼叫的方法可以使用位址的複本來存取原始位址和位址參考複本的類別成員。 如果被呼叫方法變更類別成員，則呼叫方法中的原始 class 執行個體也會變更。  
  
 下例的輸出會說明其間的差異。 因為方法使用參數中的位址尋找指定的 class 執行個體欄位，所以對方法 `ClassTaker` 的呼叫會變更 class 執行個體的 `willIChange` 欄位值。 因為引數值是 struct 本身的複本，不是其位址的複本，所以對方法 `StructTaker` 的呼叫不會變更呼叫方法中的 struct 的 `willIChange` 欄位。 `StructTaker` 變更複本，而複本在完成對 `StructTaker` 的呼叫時遺失。  
  
## <a name="example"></a>範例  
 [!code-csharp[csProgGuideObjects#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#32)]  
  
## <a name="see-also"></a>另請參閱

- [C# 程式設計指南](../index.md)
- [類別](./classes.md)
- [結構](./structs.md)
- [傳遞參數](./passing-parameters.md)
