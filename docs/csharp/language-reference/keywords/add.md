---
title: add - C# 參考
ms.custom: seodec18
ms.date: 07/20/2015
f1_keywords:
- add_CSharpKeyword
helpviewer_keywords:
- add event accessor [C#]
ms.assetid: faf30b99-10e8-45cd-ab9a-57585d4d1d8d
ms.openlocfilehash: 1cf82e3d048e465d533e87dc639a13071b41544a
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2019
ms.locfileid: "69606037"
---
# <a name="add-c-reference"></a>add (C# 參考)
使用 `add` 內容關鍵字定義自訂事件存取子，以在用戶端程式碼訂閱[事件](./event.md)時叫用。 如果提供自訂的 `add` 存取子，您也必須提供 [remove](./remove.md) 存取子。  
  
## <a name="example"></a>範例  
 下例示範具有自訂 `add` 和 [remove](./remove.md) 存取子的事件。 如需完整範例，請參閱[如何：實作介面事件](../../programming-guide/events/how-to-implement-interface-events.md)。  
  
[!code-csharp[csrefKeywordsContextual#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsContextual/CS/csrefKeywordsContextual.cs#15)]
  
 您通常不需要提供自己的自訂事件存取子。 宣告事件時編譯器自動產生的存取子，足以應付大部分的狀況。  
  
## <a name="see-also"></a>另請參閱

- [事件](../../programming-guide/events/index.md)
