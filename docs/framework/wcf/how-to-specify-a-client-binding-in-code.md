---
title: HOW TO：在程式碼中指定用戶端繫結
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 6bee5da4-adf7-42e6-8f78-63a9e5c6dbad
ms.openlocfilehash: 37769a84ca623e2f7f246d36180aa17537e90bfa
ms.sourcegitcommit: 005980b14629dfc193ff6cdc040800bc75e0a5a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2019
ms.locfileid: "70990225"
---
# <a name="how-to-specify-a-client-binding-in-code"></a>作法：在程式碼中指定用戶端繫結
在這個範例中，建立了一個使用計算機服務的用戶端，並於程式碼中以命令方式指定該用戶端的繫結。 用戶端會存取 `CalculatorService` (該服務會實作 `ICalculator` 介面)，而服務和用戶端都會使用 <xref:System.ServiceModel.BasicHttpBinding> 類別。  
  
 此程序假設計算機服務正在執行中。 如需建立服務的相關資訊， [請參閱如何：在設定中](../../../docs/framework/wcf/how-to-specify-a-service-binding-in-configuration.md)指定服務系結。 它也會使用[System.servicemodel 中繼資料公用程式工具（Svcutil）](../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)，WINDOWS COMMUNICATION FOUNDATION （WCF）提供自動產生用戶端元件。 此工具會產生存取服務所需的用戶端程式碼。  
  
 用戶端會建置成兩個部分。 Svcutil.exe 會產生 `ClientCalculator`，而該元件會實作 `ICalculator` 介面。 接著，會建構 `ClientCalculator` 的執行個體，並在程式碼中指定此服務的繫結與位址，藉此建構此用戶端應用程式。  
  
 如需此範例的來源複本，請參閱[BasicBinding](../../../docs/framework/wcf/samples/basicbinding.md)範例。  
  
### <a name="to-specify-a-custom-binding-in-code"></a>若要在程式碼中指定自訂繫結  
  
1. 從命令列使用 Svcutil.exe 產生取自服務中繼資料的程式碼。  
  
    ```console  
    Svcutil.exe <service's Metadata Exchange (MEX) address or HTTP GET address>   
    ```  
  
2. 所產生的用戶端會包含 `ICalculator` 介面，其中定義了用戶端實作所必須滿足的服務合約。  
  
     [!code-csharp[C_HowTo_CodeClientBinding#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_codeclientbinding/cs/client.cs#1)]
     [!code-vb[C_HowTo_CodeClientBinding#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_codeclientbinding/vb/client.vb#1)]  
  
3. 產生的用戶端也會包含 `ClientCalculator` 的實作。  
  
     [!code-csharp[C_HowTo_CodeClientBinding#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_codeclientbinding/cs/client.cs#2)]
     [!code-vb[C_HowTo_CodeClientBinding#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_codeclientbinding/vb/client.vb#2)]  
  
4. 建立 `ClientCalculator` 的執行個體 (此執行個體會在用戶端應用程式中使用 <xref:System.ServiceModel.BasicHttpBinding> 類別)，然後在指定的位址上呼叫服務作業。  
  
     [!code-csharp[C_HowTo_CodeClientBinding#3](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_codeclientbinding/cs/client.cs#3)]
     [!code-vb[C_HowTo_CodeClientBinding#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_codeclientbinding/vb/client.vb#3)]  
  
5. 請編譯並執行用戶端。  
  
## <a name="see-also"></a>另請參閱

- [使用繫結設定服務與用戶端](../../../docs/framework/wcf/using-bindings-to-configure-services-and-clients.md)
