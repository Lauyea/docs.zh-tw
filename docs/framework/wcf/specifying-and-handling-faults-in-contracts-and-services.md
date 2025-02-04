---
title: 指定與處理合約和服務中的錯誤
ms.date: 03/30/2017
helpviewer_keywords:
- handling faults [WCF]
ms.assetid: a9696563-d404-4905-942d-1e0834c26dea
ms.openlocfilehash: de12097f018e17b11a2beac663e0b0c51c7a2a17
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2019
ms.locfileid: "70038392"
---
# <a name="specifying-and-handling-faults-in-contracts-and-services"></a>指定與處理合約和服務中的錯誤

Windows Communication Foundation (WCF) 應用程式會藉由將 managed 例外狀況物件對應至 SOAP 錯誤物件和 SOAP 錯誤物件, 來處理錯誤情況。 本節中的主題討論如何設計合約以將錯誤條件公開為自訂 SOAP 錯誤、如何將此類錯誤當成服務實作的一部份傳回，以及用戶端如何捕捉此類錯誤。

## <a name="error-handling-overview"></a>錯誤處理概觀

在所有 Managed 應用程式中，處理錯誤由 <xref:System.Exception> 物件表示。 在以 SOAP 為基礎的應用程式 (例如 WCF 應用程式) 中, 服務方法會使用 SOAP 錯誤訊息來溝通處理錯誤資訊。 SOAP 錯誤是包含在服務作業之中繼資料內的訊息類型，因此會建立錯誤合約，而用戶端可使用該合約來讓其作業更穩定或更具互動性。 此外, 因為 SOAP 錯誤是以 XML 格式呈現給用戶端, 所以它是一種高度互通的型別系統, 可讓用戶端在任何 SOAP 平臺上使用, 進而增加 WCF 應用程式的範圍。

因為 WCF 應用程式會在這兩種類型的錯誤系統下執行, 所以任何傳送至用戶端的 managed 例外狀況資訊都必須從例外狀況轉換為服務上的 SOAP 錯誤、傳送, 然後從 SOAP 錯誤轉換為 WCF 用戶端中的錯誤例外狀況。 在雙工用戶端的情況當中，用戶端合約也可能會將 SOAP 錯誤傳回服務。 不管哪種情況，您都可以使用預設的服務例外狀況行為，或明確地控制是否要將例外狀況對應到錯誤訊息，以及其對應方式。

可以傳送兩種類型的 SOAP 錯誤:宣告和未宣告。 已宣告的 SOAP 錯誤是其中作業具有指定自訂 SOAP 錯誤類型之 <xref:System.ServiceModel.FaultContractAttribute?displayProperty=nameWithType> 屬性的 SOAP 錯誤。 未宣告作業的合約中未指定 SOAP 錯誤。

我們強烈建議，服務作業應使用 <xref:System.ServiceModel.FaultContractAttribute> 屬性正式指定用戶端在正常作業期間可能收到的所有 SOAP 錯誤，以宣告其錯誤。 我們也建議您只在 SOAP 錯誤中傳回用戶端應該知道的資訊，將資訊暴露的程度降至最低。

一般來說，服務 (與雙工用戶端) 會採取下列步驟，成功地將錯誤處理整合到應用程式中：

- 將例外狀況條件對應至自訂 SOAP 錯誤。

- 用戶端與服務會將 SOAP 錯誤當成例外狀況來傳送與接收。

此外, WCF 用戶端和服務可以使用未宣告的 soap 錯誤進行調試, 並且可以擴充預設的錯誤行為。 下列各節將說明這些工作與概念。

## <a name="map-exceptions-to-soap-faults"></a>將例外狀況對應至 SOAP 錯誤

建立可以處理錯誤情況的作業的第一步，就是決定用戶端應用程式在哪種情況下應該收到有關錯誤的通知。 某些作業會有專屬自身功能的一些錯誤情況。 例如，`PurchaseOrder` 作業可能會將特定資訊傳回給已經無法再初始化採購單的客戶。 在其他情況中 (例如 `Calculator` 服務)，更常見的 `MathFault` SOAP 錯誤也許能夠說明整個服務的所有錯誤情況。 一旦識別出您服務用戶端的錯誤情況，就可以建構自訂 SOAP 錯誤，並在引發 SOAP 錯誤的對應錯誤情況時將作業標示為傳回該 SOAP 錯誤。

如需有關開發服務或用戶端的這個步驟的詳細資訊, 請參閱[定義和指定錯誤](../../../docs/framework/wcf/defining-and-specifying-faults.md)。

## <a name="clients-and-services-handle-soap-faults-as-exceptions"></a>用戶端與服務會將 SOAP 錯誤當成例外狀況來處理。

在 WCF 應用程式中成功處理錯誤的第一個步驟, 是識別作業錯誤狀況、定義自訂 SOAP 錯誤, 並將這些作業標示為傳回這些錯誤。 下一步則是適當地實作這些錯誤的傳送與接收作業。 一般來說，服務會傳送錯誤以通知用戶端應用程式有關錯誤的情況，但是雙工用戶端可以同時將 SOAP 錯誤傳送給服務。

如需詳細資訊, 請參閱傳送[和接收錯誤](../../../docs/framework/wcf/sending-and-receiving-faults.md)。

## <a name="undeclared-soap-faults-and-debugging"></a>未宣告的 SOAP 錯誤與偵錯

已宣告的 SOAP 錯誤非常適合用來建置穩固、可互通，與分散式應用程式。 但是，在某些情況下則適合透過服務 (或雙工用戶端) 來傳送未宣告的 SOAP 錯誤，此錯誤並未在該作業的 Web 服務描述語言 (WSDL) 中提及。 例如，當您開發服務時，一旦碰到未預期的情況，就可以將資訊傳回用戶端以便進行偵錯。 此外, 您可以將<xref:System.ServiceModel.ServiceBehaviorAttribute.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType>屬性<xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType>或屬性設定為`true` , 以允許 WCF 用戶端取得內部服務作業例外狀況的相關資訊。 傳送[和接收錯誤](../../../docs/framework/wcf/sending-and-receiving-faults.md)中會描述傳送個別錯誤和設定調試行為屬性。

> [!IMPORTANT]
> 因為 managed 例外狀況可以公開內部應用程式資訊<xref:System.ServiceModel.ServiceBehaviorAttribute.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType> , <xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType>所以`true`將或設為可允許 WCF 用戶端取得內部服務作業例外狀況的相關資訊, 包括個人可識別或其他機密資訊。
>
> 因此，若您只是暫時對服務應用程式進行偵錯，才建議把 <xref:System.ServiceModel.ServiceBehaviorAttribute.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType> 或 <xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType> 設為 `true`。 此外，若某個方法以這種方式傳回未處理的 Managed 例外狀況，則該方法的 WSDL 不會包含 <xref:System.ServiceModel.FaultException%601> 型別之 <xref:System.ServiceModel.ExceptionDetail> 的合約。 用戶端必須預期會有未知的 SOAP 錯誤 (以物件的形式<xref:System.ServiceModel.FaultException?displayProperty=nameWithType>傳回給 WCF 用戶端), 才能正確取得偵錯工具資訊。

## <a name="customizing-error-handling-with-ierrorhandler"></a>使用 IErrorHandler 自訂錯誤處理

如果您有特殊需求，需要在發生應用程式層級例外狀況時自訂給用戶端的回應訊息，或是在傳回回應訊息後執行某些自訂處理，請實作 <xref:System.ServiceModel.Dispatcher.IErrorHandler?displayProperty=nameWithType> 介面。

## <a name="fault-serialization-issues"></a>錯誤序列化問題

還原序列化錯誤合約時，WCF 會先嘗試使用錯誤合約型別來比對 SOAP 訊息中的錯誤合約名稱。 如果它找不到完全符合的項目，就會按照字母順序，在可用的錯誤合約清單中搜尋相容型別。 如果有兩個錯誤合約是相容型別 (例如，其中一個合約是另一個合約的子類別)，可能會使用錯誤的型別來還原序列化錯誤合約。 只有當錯誤合約沒有指定名稱、命名空間和動作時，才會發生這種情況。 若要避免此問題發生，請一律指定名稱、命名空間和動作屬性，藉以完整限定錯誤合約。 此外，如果您已經定義許多衍生自共用基底類別的相關錯誤合約，請務必使用 `[DataMember(IsRequired=true)]` 來標記任何新成員。 如需這個 `IsRequired` 屬性的詳細資訊，請參閱 <xref:System.Runtime.Serialization.DataMemberAttribute>。 這個屬性可避免基底類別成為相容型別，並且強制將錯誤合約還原序列化成正確的衍生型別。

## <a name="see-also"></a>另請參閱

- <xref:System.ServiceModel.FaultException>
- <xref:System.ServiceModel.FaultContractAttribute>
- <xref:System.ServiceModel.FaultException>
- <xref:System.Xml.Serialization.XmlSerializer>
- <xref:System.ServiceModel.XmlSerializerFormatAttribute>
- <xref:System.ServiceModel.FaultContractAttribute>
- <xref:System.ServiceModel.CommunicationException>
- <xref:System.ServiceModel.FaultContractAttribute.Action%2A>
- <xref:System.ServiceModel.FaultException.Code%2A>
- <xref:System.ServiceModel.FaultException.Reason%2A>
- <xref:System.ServiceModel.FaultCode.SubCode%2A>
- <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A>
- [定義並指定錯誤](../../../docs/framework/wcf/defining-and-specifying-faults.md)
