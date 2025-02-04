---
title: 作法：將組件載入應用程式定義域
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- application domains, loading assemblies
- loading assemblies
ms.assetid: 1432aa2d-bd83-4346-bf3b-a1b7920e2aa9
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 86b66d0a88864188d67aab19de67aaa857a06eaa
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2019
ms.locfileid: "71053174"
---
# <a name="how-to-load-assemblies-into-an-application-domain"></a>HOW TO：將組件載入應用程式定義域
有數種方式可以將組件載入應用程式定義域。 建議的方法是使用 <xref:System.Reflection.Assembly?displayProperty=nameWithType> 類別的 `static` (在 Visual Basic 中為 `Shared`) <xref:System.Reflection.Assembly.Load%2A> 方法。 其他可以載入組件的方式包括：  
  
- <xref:System.Reflection.Assembly> 類別的 <xref:System.Reflection.Assembly.LoadFrom%2A> 方法會載入已指定其檔案位置的組件。 使用這種方法載入組件會使用不同的載入內容。  
  
- <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A> 和 <xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom%2A> 方法將組件載入僅限反映的內容。 可以檢查但不會執行載入此內容的組件，以允許檢查以其他平台為目標的組件。 請參閱[How to:將組件載入僅限反映的內容](../reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md)。  
  
> [!NOTE]
> 僅限反映的內容是 .NET Framework 2.0 版的新功能。  
  
- <xref:System.AppDomain> 類別的 <xref:System.AppDomain.CreateInstance%2A> 和 <xref:System.AppDomain.CreateInstanceAndUnwrap%2A> 這類方法可以將組件載入應用程式定義域。  
  
- <xref:System.Type> 類別的 <xref:System.Type.GetType%2A> 方法可以載入組件。  
  
- <xref:System.AppDomain?displayProperty=nameWithType> 類別的 <xref:System.AppDomain.Load%2A> 方法可以載入組件，但主要用於 COM 互通性。 它不應該用來將組件載入其呼叫所在應用程式定義域以外的應用程式定義域。  
  
> [!NOTE]
> 從 .NET Framework 2.0 版開始，執行階段不會載入使用 .NET Framework 版本所編譯的組件，而這個版本的版本號碼高於目前載入的執行階段。 這適用於版本號碼的主要與次要元件組合。  
  
 您可以指定在應用程式定義域之間共用所載入組件之 Just-In-Time (JIT) 編譯程式碼的方式。 如需詳細資訊，請參閱[應用程式定義域和組件](application-domains.md#application-domains-and-assemblies)。  
  
## <a name="example"></a>範例  
 下列程式碼會將名為 "example.exe" 或 "example.dll" 的組件載入目前應用程式定義域、從組件取得名為 `Example` 的類型、取得適用於該類型且名為 `MethodA` 的無參數方法，並且執行方法。 如需從載入的組件取得資訊的完整討論，請參閱[動態載入和使用類型](../reflection-and-codedom/dynamically-loading-and-using-types.md)。  
  
 [!code-cpp[System.AppDomain.Load#2](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.appdomain.load/cpp/source2.cpp#2)]
 [!code-csharp[System.AppDomain.Load#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.load/cs/source2.cs#2)]
 [!code-vb[System.AppDomain.Load#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.load/vb/source2.vb#2)]  
  
## <a name="see-also"></a>另請參閱

- <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A>
- [使用應用程式定義域設計程式](application-domains.md#programming-with-application-domains)
- [反映](../reflection-and-codedom/reflection.md)
- [使用應用程式定義域](use.md)
- [如何：將組件載入到僅限反映的內容將組件載入到僅限反映的內容](../reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md)
- [應用程式定義域和組件](application-domains.md#application-domains-and-assemblies)
