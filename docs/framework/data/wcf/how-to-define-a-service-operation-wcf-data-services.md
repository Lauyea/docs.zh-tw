---
title: 作法：定義服務作業（WCF Data Services）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Service Operations [WCF Data Services]
- WCF Data Services, service operations
ms.assetid: dfcd3cb1-2f07-4d0b-b16a-6b056c4f45fa
ms.openlocfilehash: 3154fadeda400440f68a184b430b7ff15a02203d
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70780082"
---
# <a name="how-to-define-a-service-operation-wcf-data-services"></a>作法：定義服務作業（WCF Data Services）

[!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] 會將伺服器上定義的方法公開為服務作業。 服務作業可讓資料服務透過 URI 提供對伺服器上定義之方法的存取權。 若要定義服務作業，請將 [`WebGet]`或`[WebInvoke]`屬性套用至方法。 若要支援查詢運算子，服務作業必須<xref:System.Linq.IQueryable%601>傳回實例。 服務作業可以透過 <xref:System.Data.Services.DataService%601.CurrentDataSource%2A> 上的 <xref:System.Data.Services.DataService%601> 屬性存取基礎資料資源。 如需詳細資訊，請參閱[服務作業](service-operations-wcf-data-services.md)。

本主題的範例定義名為 `GetOrdersByCity` 的服務作業，此服務作業會針對 <xref:System.Linq.IQueryable%601> 執行個體傳回篩選過的 `Orders`，以及相關的 `Order_Details` 物件。 此範例存取的 <xref:System.Data.Objects.ObjectContext> 執行個體為 Northwind 範例資料服務的資料來源。 此服務會在您完成[WCF Data Services 快速入門](quickstart-wcf-data-services.md)時建立。

### <a name="to-define-a-service-operation-in-the-northwind-data-service"></a>在 Northwind 資料服務中定義服務作業

1. 在 Northwind 資料服務專案中，開啟 Northwind.svc 檔案。

2. 在 `Northwind` 類別中，定義名為 `GetOrdersByCity` 的服務作業方法，如下所示：

     [!code-csharp[Astoria Northwind Service#ServiceOperationDef](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_service/cs/northwind2.svc.cs#serviceoperationdef)]
     [!code-vb[Astoria Northwind Service#ServiceOperationDef](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_service/vb/northwind2.svc.vb#serviceoperationdef)]

3. 在 `InitializeService` 類別的`Northwind` 方法中加入下列程式碼，以存取服務作業：

     [!code-csharp[Astoria Northwind Service#ServiceOperationConfig](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_service/cs/northwind2.svc.cs#serviceoperationconfig)]
     [!code-vb[Astoria Northwind Service#ServiceOperationConfig](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_service/vb/northwind2.svc.vb#serviceoperationconfig)]

### <a name="to-query-the-getordersbycity-service-operation"></a>查詢 GetOrdersByCity 服務作業

- 在 Web 瀏覽器中輸入下列其中一個 URI，叫用在下列範例中定義的服務作業：

  - `http://localhost:12345/Northwind.svc/GetOrdersByCity?city='London'`

  - `http://localhost:12345/Northwind.svc/GetOrdersByCity?city='London'&$top=2`

  - `http://localhost:12345/Northwind.svc/GetOrdersByCity?city='London'&$expand=Order_Details&$orderby=RequiredDate desc`

## <a name="example"></a>範例

下列範例會在 Northwind 資料服務實作名為 `GetOrderByCity` 的服務作業。 此作業使用 ADO.NET Entity Framework，根據所提供的城市名稱傳回一組 `Orders` 和相關的 `Order_Details` 物件，做為 <xref:System.Linq.IQueryable%601> 執行個體。

> [!NOTE]
> 此服務作業端點支援查詢運算子，因為方法會傳回 <xref:System.Linq.IQueryable%601> 執行個體。

[!code-csharp[Astoria Northwind Service#ServiceOperation](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_service/cs/northwind2.svc.cs#serviceoperation)]
[!code-vb[Astoria Northwind Service#ServiceOperation](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_service/vb/northwind2.svc.vb#serviceoperation)]

## <a name="see-also"></a>另請參閱

- [定義 WCF Data Services](defining-wcf-data-services.md)
