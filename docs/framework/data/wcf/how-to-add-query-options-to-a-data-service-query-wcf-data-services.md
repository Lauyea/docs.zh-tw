---
title: 作法：將查詢選項加入至資料服務查詢（WCF Data Services）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- querying the data service [WCF Data Services]
- WCF Data Services, querying
- WCF Data Services, accessing data
ms.assetid: e4258526-557e-4e96-91e1-2175400c7c8f
ms.openlocfilehash: f7b0557938d1419b79c3191cf8f9110cab2f5ce6
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70790777"
---
# <a name="how-to-add-query-options-to-a-data-service-query-wcf-data-services"></a>作法：將查詢選項加入至資料服務查詢（WCF Data Services）
[!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] 可讓您使用所產生的用戶端資料服務類別，從以 .NET Framework 為基礎的用戶端應用程式查詢資料服務。 要執行此作業之最簡易的方式是撰寫 Language Integrated Query (LINQ) 查詢運算式，其中要包含想要的查詢選項。 您也可以呼叫一系列的 LINQ 查詢方法來撰寫功能相等的查詢。 最後，您可以使用 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> 方法，在查詢中加入查詢選項。 在每個案例中，用戶端產生的 URI 都會包括所要求的實體集，以及選取的已套用之查詢選項。 如需詳細資訊，請參閱[查詢資料服務](querying-the-data-service-wcf-data-services.md)。  
  
 本主題中的範例使用 Northwind 範例資料服務和自動產生的用戶端資料服務類別。 當您完成[WCF Data Services 快速入門](quickstart-wcf-data-services.md)時，會建立此服務和用戶端資料類別。  
  
## <a name="example"></a>範例  
 下列範例示範如何撰寫 LINQ 查詢運算式，該運算式只會傳回運費超過 $30 美元的訂單，並按出貨日期遞減排序結果。  
  
 [!code-csharp[Astoria Northwind Client#AddQueryOptionsLinq](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionslinq)]
 [!code-vb[Astoria Northwind Client#AddQueryOptionsLinq](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionslinq)]  
  
## <a name="example"></a>範例  
 下列範例示範如何利用 LINQ 查詢方法撰寫相當於前一個查詢的 LINQ 查詢。  
  
 [!code-csharp[Astoria Northwind Client#AddQueryOptionsLinqExpression](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionslinqexpression)]
 [!code-vb[Astoria Northwind Client#AddQueryOptionsLinqExpression](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionslinqexpression)]  
  
## <a name="example"></a>範例  
 下列範例示範如何使用 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> 方法來建立相當於前面範例的 <xref:System.Data.Services.Client.DataServiceQuery%601>。  
  
 [!code-csharp[Astoria Northwind Client#AddQueryOptions](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptions)]
 [!code-vb[Astoria Northwind Client#AddQueryOptions](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptions)]  
  
## <a name="example"></a>範例  
 下列範例示範如何使用 `$orderby` 查詢選項按 Freight 屬性篩選並排序傳回的 Orders 物件。  
  
 [!code-csharp[Astoria Northwind Client#OrderWithFilter](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#orderwithfilter)]
 [!code-vb[Astoria Northwind Client#OrderWithFilter](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#orderwithfilter)]  
  
## <a name="see-also"></a>另請參閱

- [查詢資料服務](querying-the-data-service-wcf-data-services.md)
- [如何：專案查詢結果](how-to-project-query-results-wcf-data-services.md)
