---
title: WCF Data Services 通訊協定實作詳細資訊
ms.date: 03/30/2017
ms.assetid: 712d689b-fada-4cbb-bcdb-d65a3ef83b4c
ms.openlocfilehash: 1cd4204d9e1626ac45bccf3954fdaf1c156af7f8
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70779650"
---
# <a name="wcf-data-services-protocol-implementation-details"></a>WCF Data Services 通訊協定實作詳細資訊
## <a name="odata-protocol-implementation-details"></a>OData 通訊協定實作詳細資訊  
 [!INCLUDE[ssODataFull](../../../../includes/ssodatafull-md.md)] 要求實作通訊協定的資料服務提供一組最低限度的特定功能。 這些功能在通訊協定檔中是以「應該」和「必須」描述。 其他選用的功能會以「可能」的角度來說明。 本主題描述 [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] 目前尚未實作的選擇性功能。 如需詳細資訊，請參閱[OData 通訊協定檔](https://go.microsoft.com/fwlink/?LinkID=184554)。  
  
### <a name="support-for-the-format-query-option"></a>支援 $format 查詢選項  
 The [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)] 通訊協定同時支援 JavaScript 標記法 (JSON) 和 Atom 摘要，而且 [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)] 提供 `$format` 系統查詢選項，讓用戶端要求回應摘要的格式。 此系統查詢選項 (如果資料服務支援) 必須覆寫要求之 Accept 標頭的值。 [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] 支援同時傳回 JSON 和 Atom 摘要。 不過，預設實作不支援 `$format` 查詢選項，而且僅使用 Accept 標頭的值決定回應的格式。 在某些情況下，您的資料服務可能需要支援 `$format` 查詢選項，例如，用戶端無法設定 Accept 標頭時。 為支援這些案例，您必須擴充資料服務來處理 URI 中的這個選項。 您可以從 MSDN 程式碼庫網站下載[JSONP 和 URL 控制的 ADO.NET 資料服務範例專案格式支援](https://go.microsoft.com/fwlink/?LinkId=208228)，將此功能新增至您的資料服務，並將它新增至您的資料服務專案。 此範例會移除 `$format` 查詢選項，並將 Accept 標頭變更為 `application/json`。 當您加入範例專案時，將 `JSONPSupportBehaviorAttribute` 加入至您的資料服務類別中可讓服務處理 `$format` 查詢選項 `$format=json`。 如果也要處理 `$format=atom` 或其他自訂格式，則需要進一步自訂這個範例專案。  
  
## <a name="wcf-data-services-behaviors"></a>WCF Data Services 行為  
 下列 [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] 行為未透過 [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)] 通訊協定明確定義：  
  
### <a name="default-sorting-behavior"></a>預設排序行為  
 當傳送至資料服務的查詢要求包含 `$top` 或 `$skip` 系統查詢選項，但不包含 `$orderby` 系統查詢選項時，傳回的摘要會依索引鍵屬性，以遞增順序排序。 這是因為您需要排序，才能確保結果分頁正確。 若要這樣做，資料服務要將排序運算式加入至查詢中。 在資料服務中啟用伺服器驅動型分頁時，也會發生這個行為。 如需詳細資訊，請參閱設定[資料服務](configuring-the-data-service-wcf-data-services.md)。若要控制傳回摘要的順序，您應該在查詢`$orderby` URI 中包含。  
  
## <a name="see-also"></a>另請參閱

- [定義 WCF Data Services](defining-wcf-data-services.md)
- [WCF Data Services 用戶端程式庫](wcf-data-services-client-library.md)
