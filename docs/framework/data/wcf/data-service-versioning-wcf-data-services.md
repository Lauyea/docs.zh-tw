---
title: 資料服務版本控制 (WCF Data Services)
ms.date: 03/30/2017
helpviewer_keywords:
- versioning, WCF Data Services
- versioning [WCF Data Services]
- WCF Data Services, versioning
ms.assetid: e3e899cc-7f25-4f67-958f-063f01f79766
ms.openlocfilehash: f2007e5c2fa638d64c5c1e0d6879e12c7bcc901d
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70854095"
---
# <a name="data-service-versioning-wcf-data-services"></a>資料服務版本控制 (WCF Data Services)
可[!INCLUDE[ssODataFull](../../../../includes/ssodatafull-md.md)]讓您建立資料服務，讓用戶端可以使用以資料模型為基礎的 uri，將資料當做資源來存取。 OData 也支援服務作業的定義。 初始部署並在其存留期期間潛在進行數次之後，可能會因為各種原因而需要變更這些資料服務 (例如變更商務需要、資訊技術需求) 或處理其他問題。 當您針對現有的資料服務進行變更時，必須考慮是否要定義新的資料服務版本，以及如何妥善地將對於現有用戶端應用程式的影響降至最低。 本主題提供建立新資料服務版本時機和方式的指引。 它也會說明 WCF Data Services 如何處理用戶端與資料服務之間的交換，以支援不同版本的 OData 通訊協定。

## <a name="versioning-a-wcf-data-service"></a>WCF Data Services 的版本控制
 部署資料服務並取用資料之後，對於資料服務的變更便有可能造成與現有用戶端應用程式的相容性問題。 不過，由於服務的整體商務需求通常需要變更，因此您必須考慮建立新資料服務版本的時機與方式，並將對於用戶端應用程式的影響降至最低。

### <a name="data-model-changes-that-recommend-a-new-data-service-version"></a>建議新資料服務版本的資料模型變更
 考慮是否要發行新資料服務版本時，最好先了解不同種類的變更影響用戶端應用程式的方式。 對於可能要求您建立新資料服務版本之資料服務的變更可分成下列兩種類別：

- 對於服務合約的變更—其中包括服務作業的更新、對於實體集 (摘要) 存取範圍的變更、版本變更，以及對於服務行為的其他變更。

- 對於資料合約的變更—其中包括對於資料模型、摘要格式或摘要自訂的變更。

 下表詳細說明您應該考慮發行新資料服務版本的哪些變更種類：

|變更類型|需要新版本|不需要新版本|
|--------------------|----------------------------|----------------------------|
|服務作業|-加入新參數<br />-變更傳回類型<br />-移除服務作業|-刪除現有的參數<br />-新增服務作業|
|服務行為|-停用計數要求<br />-停用投射支援<br />-增加所需的資料服務版本|-啟用計數要求<br />-啟用投影支援<br />-減少所需的資料服務版本|
|實體集權限|-限制實體集許可權<br />-變更回應碼（新的第一個數位值） <sup>1</sup>|-放寬實體集許可權<br />-變更回應碼（相同的第一個數位值）|
|實體屬性|-移除現有的屬性或關聯性<br />-加入不可為 null 的屬性<br />-變更現有屬性|-加入可為 null 的屬性<sup>2</sup>|
|實體集|-移除實體集|-加入衍生類型<br />-變更基底類型<br />-新增實體集|
|摘要自訂|-變更實體屬性對應||

 <sup>1</sup>這可能取決於用戶端應用程式依賴接收特定錯誤碼的嚴格程度。

 <sup>2</sup>您可以將<xref:System.Data.Services.Client.DataServiceContext.IgnoreMissingProperties%2A>屬性設定為`true` ，讓用戶端忽略用戶端上未定義的資料服務所傳送的任何新屬性。 不過，進行插入時，用戶端沒有包含在 POST 要求中的屬性會設為其預設值。 若是更新，屬性中用戶端未知的任何現有資料都可能會以預設值覆寫。 在此情況下，您應該將更新當做 MERGE 要求傳送，這是預設值。 如需詳細資訊，請參閱[管理資料服務內容](managing-the-data-service-context-wcf-data-services.md)。

### <a name="how-to-version-a-data-service"></a>如何進行資料服務的版本控制
 必要時，您可以使用更新的服務合約或資料模型建立新的服務執行個體，藉以定義新的資料服務版本。 接著，您要使用新的 URI 端點公開這個新服務，以區別舊版本。 例如：

- 舊版本：`http://services.odata.org/Northwind/v1/Northwind.svc/`

- 新版本：`http://services.odata.org/Northwind/v2/Northwind.svc/`

 升級資料服務時，用戶端也需要根據新的資料服務中繼資料進行更新，並使用新的根 URI。 如果可能，您應該維護資料服務的舊版本，以支援尚未進行升級以使用新版本的用戶端。 不再需要舊版本的資料服務時，可以將其移除。 您應該考慮在外部組態檔中維護資料服務端點 URI。

## <a name="odata-protocol-versions"></a>OData 通訊協定版本
 發行新版本的 OData 時，用戶端應用程式可能不會使用資料服務所支援的相同 OData 通訊協定版本。 較舊的用戶端應用程式可能會存取支援較新版本 OData 的資料服務。 用戶端應用程式也可以使用較新版本的 WCF Data Services 用戶端程式庫，其支援的 OData 版本比存取的資料服務還新。

 WCF Data Services 利用 OData 提供的支援來處理這類版本控制案例。 此外，當用戶端使用的 OData 版本與資料服務所使用的不同時，也支援產生及使用資料模型中繼資料來建立用戶端資料服務類別。 如需詳細資訊， [請參閱 OData：通訊協定](https://go.microsoft.com/fwlink/?LinkId=186071)版本設定。

### <a name="version-negotiation"></a>版本交涉
 您可以設定資料服務，以定義服務將使用的最高版本 OData 通訊協定，而不論用戶端要求的版本為何。 若要這麼做，您可以<xref:System.Data.Services.Common.DataServiceProtocolVersion> <xref:System.Data.Services.DataServiceBehavior.MaxProtocolVersion%2A>為數據服務所使用之<xref:System.Data.Services.DataServiceBehavior>的屬性指定值。 如需詳細資訊，請參閱設定[資料服務](configuring-the-data-service-wcf-data-services.md)。

 當應用程式使用 WCF Data Services 用戶端程式庫來存取資料服務時，程式庫會根據您應用程式中所使用的 OData 版本和功能，自動將這些標頭設定為正確的值。 根據預設，WCF Data Services 會使用支援所要求作業的最低通訊協定版本。

 下表詳細說明包含特定版本 OData 通訊協定 WCF Data Services 支援的 .NET Framework 和 Silverlight 版本。

|OData 通訊協定版本|下列版本引入的支援|
|-----------------------------------------------------------------------------------|----------------------------|
|第 1 版|-   [!INCLUDE[netfx35_long](../../../../includes/netfx35-long-md.md)]Service Pack 1 （SP1）<br />-Silverlight 第3版|
|第 2 版|-   [!INCLUDE[netfx40_long](../../../../includes/netfx40-long-md.md)]<br />- [!INCLUDE[netfx35_long](../../../../includes/netfx35-long-md.md)] SP1 的更新。 您可以從[Microsoft 下載中心](https://go.microsoft.com/fwlink/?LinkId=158125)下載並安裝更新。<br />-Silverlight 第4版|
|第 3 版|-您可以從[Microsoft 下載中心](https://go.microsoft.com/fwlink/?LinkId=203885)下載並安裝支援 OData 第3版的發行前版本。|

### <a name="metadata-versions"></a>中繼資料版本
 根據預設，WCF Data Services 會使用1.1 版的 CSDL 來代表資料模型。 這就是以反映提供者或自訂資料服務提供者為基礎之資料模型的情況。 但是，當資料模型使用 Entity Framework 來定義時，傳回的 CSDL 的版本會與 Entity Framework 所使用的版本相同。 CSDL 的版本取決於[Schema 元素（CSDL）](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#schema-element-csdl)的命名空間。

 傳回之中繼資料的 `DataServices` 項目還包含 `DataServiceVersion` 屬性，該值與回應訊息中 `DataServiceVersion` 標頭的值相同。 用戶端應用程式（例如 Visual Studio 中的 [**加入服務參考**] 對話方塊）會使用這項資訊來產生用戶端資料服務類別，以便與裝載資料服務的 WCF Data Services 版本正確搭配運作。 如需詳細資訊， [請參閱 OData：通訊協定](https://go.microsoft.com/fwlink/?LinkId=186071)版本設定。

## <a name="see-also"></a>另請參閱

- [資料服務提供者](data-services-providers-wcf-data-services.md)
- [定義 WCF Data Services](defining-wcf-data-services.md)
