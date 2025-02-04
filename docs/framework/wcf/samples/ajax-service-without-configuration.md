---
title: 無組態的 AJAX 服務
ms.date: 03/30/2017
ms.assetid: e6db7acd-5679-45d4-b98a-8449c6873838
ms.openlocfilehash: 06af14ad551de0e56700b044aea25b59dbf890ce
ms.sourcegitcommit: 5ae5a1a9520b8b8b6164ad728d396717f30edafc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/11/2019
ms.locfileid: "70895125"
---
# <a name="ajax-service-without-configuration"></a>無組態的 AJAX 服務

這個範例示範如何使用 Windows Communication Foundation （WCF）來建立基本的 ASP.NET 非同步 JavaScript 和 XML （AJAX）服務（您可以從網頁瀏覽器用戶端使用 JavaScript 程式碼存取的服務），而不需使用任何設定設置。 此服務會在 .svc 檔中使用特殊語法以自動啟用 AJAX 端點。

WCF 中的 AJAX 支援已優化，可透過`ScriptManager`控制項與 ASP.NET ajax 搭配使用。 如需搭配使用 WCF 與 ASP.NET AJAX 的範例，請參閱[AJAX 範例](ajax.md)。

> [!NOTE]
> 此範例的安裝程序與建置指示位於本主題的結尾。

 這個範例是以使用 HTTP POST 的 AJAX 服務為基礎所建立。 如[基本 AJAX 服務](../../../../docs/framework/wcf/samples/basic-ajax-service.md)範例中所述， <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory>是用來裝載服務。

```text
<%ServiceHost
    language=c#
    Debug="true"
    Service="Microsoft.Ajax.Samples.CalculatorService
    Factory="System.ServiceModel.Activation.WebScriptServiceHostFactory"
%>
```

<xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> 會自動將 <xref:System.ServiceModel.Description.WebScriptEndpoint> 加入至服務。 如果不需要對端點進行任何組態變更，您就可以從服務的 Web.config 檔案中完全移除 `<system.ServiceModel>` 區段。 Web.config 檔案會包含 ConfigFreeClientPage.aspx 所使用的一些 ASP.NET 設定。 如果沒有，就可以移除整個 Web.config 檔案。

> [!IMPORTANT]
> 這些範例可能已安裝在您的電腦上。 請先檢查下列 (預設) 目錄，然後再繼續。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目錄不存在, 請移至[.NET Framework 4 的 Windows Communication Foundation (wcf) 和 Windows Workflow Foundation (WF) 範例](https://go.microsoft.com/fwlink/?LinkId=150780), 以下載所有 Windows Communication Foundation (wcf) [!INCLUDE[wf1](../../../../includes/wf1-md.md)]和範例。 此範例位於下列目錄。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Ajax\ConfigFreeAjaxService`

#### <a name="to-set-up-build-and-run-the-sample"></a>若要安裝、建置及執行範例

1. 請確定您在[Windows Communication Foundation 範例的一次性設定程式](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)中執行設定指示。

2. 如[建立 Windows Communication Foundation 範例](../../../../docs/framework/wcf/samples/building-the-samples.md)中所述，建立方案 ConfigFreeAjaxService。

3. 流覽至`http://localhost/ServiceModelSamples/ConfigFreeClientPage.aspx` （不要在瀏覽器中從專案目錄開啟 configfreeclientpage.aspx）。

> [!NOTE]
> 執行這個範例時，請確定 IIS 中 ServiceModelSamples 資料夾的匿名驗證與 Windows 驗證並未同時啟用。 如果是這種情況，請停用 Windows 驗證。 在執行範例之後，請啟用 Windows 驗證並執行「iisreset」。

## <a name="see-also"></a>另請參閱

- [基本 AJAX 服務](../../../../docs/framework/wcf/samples/basic-ajax-service.md)
