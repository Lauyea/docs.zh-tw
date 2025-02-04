---
title: 資料服務提供者 (WCF 資料服務)
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, providers
ms.assetid: a0160b1b-3d9c-4cc8-8391-cb0986a60a41
ms.openlocfilehash: 7623c2b743d6a61362c8cf0e1228b4663c9e7d48
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70780383"
---
# <a name="data-services-providers-wcf-data-services"></a>資料服務提供者 (WCF 資料服務)
[!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)]支援將[!INCLUDE[ssODataFull](../../../../includes/ssodatafull-md.md)]資料公開為摘要的多個提供者模型。 本主題所提供的資訊能讓您針對您的資料來源，選擇最適合的 [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] 提供者。  
  
## <a name="data-source-providers"></a>資料來源提供者  
 [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)]支援下列提供者以定義資料服務的資料模型。  
  
|提供者|說明|  
|--------------|-----------------|  
|Entity Framework 提供者|此提供者使用 ADO.NET Entity Framework 定義對應至關聯式資料的資料模型，讓您能夠使用關聯式資料與資料服務。 您的資料來源可以是 SQL Server，或者是任何其他具備 Entity Framework 協力廠商提供者支援的資料來源。 如果您擁有關聯式資料來源 (例如 SQL Server 資料庫)，您應該使用 Entity Framework 提供者。 如需詳細資訊，請參閱[Entity Framework 提供者](entity-framework-provider-wcf-data-services.md)。|  
|反映提供者|此提供者會使用反應，讓您能夠根據現有的資料類別定義資料模型 (可公開為 <xref:System.Linq.IQueryable%601> 介面的執行個體)。 實作 <xref:System.Data.Services.IUpdatable> 介面即可進行更新。 如果您擁有在執行階段定義的靜態資料類別，例如 LINQ to SQL 所產生的資料類別或是由具型別 DataSet 定義的資料類別，您應該使用此提供者。 如需詳細資訊，請參閱[反映提供者](reflection-provider-wcf-data-services.md)。|  
|自訂資料服務提供者|[!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] 包含一組提供者，可讓您根據晚期繫結的資料型別動態定義資料模型。 當應用程式正在設計時公開的資料未知，或是 Entity Framework 或反射提供者不夠時，您應該實作這些介面。 如需詳細資訊，請參閱[自訂資料服務提供者](custom-data-service-providers-wcf-data-services.md)。|  
  
## <a name="other-data-service-providers"></a>其他資料服務提供者  
 [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)]具有下列額外的資料服務提供者，可增強使用其中一個其他提供者所定義之資料來源的效能。  
  
|提供者|描述|  
|--------------|-----------------|  
|資料流處理提供者|此提供者可讓您使用 [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)]，以公開二進位大型物件資料型別。 資料流處理提供者是透過實作 <xref:System.Data.Services.Providers.IDataServiceStreamProvider> 介面而建立的。 這個提供者可以與任何資料來源提供者一起實作。 如需詳細資訊，請參閱[串流處理提供者](streaming-provider-wcf-data-services.md)。|  
  
## <a name="see-also"></a>另請參閱

- [定義 WCF Data Services](defining-wcf-data-services.md)
- [設定資料服務](configuring-the-data-service-wcf-data-services.md)
- [裝載資料服務](hosting-the-data-service-wcf-data-services.md)
