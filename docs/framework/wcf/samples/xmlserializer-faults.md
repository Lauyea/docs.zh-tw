---
title: XmlSerializer 錯誤
ms.date: 03/30/2017
ms.assetid: c6b80f14-64f4-4162-ae76-71664cf42fd3
ms.openlocfilehash: 5375f6c073ef76309ad36c68c4d73ec2a7b9fca9
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2019
ms.locfileid: "70044495"
---
# <a name="xmlserializer-faults"></a>XmlSerializer 錯誤
<xref:System.Xml.Serialization.XmlSerializer> 錯誤合約範例會使用 <xref:System.Xml.Serialization.XmlSerializer> 示範如何在服務與用戶端之間傳達錯誤資訊。 此範例是以[消費者入門](../../../../docs/framework/wcf/samples/getting-started-sample.md)為基礎, 並在服務中加入一些額外的程式碼, 以將內部例外狀況轉換為錯誤。 用戶端會嘗試執行除數為零，以便強制在服務上造成錯誤狀況。  
  
> [!NOTE]
> 此範例的安裝程序與建置指示位於本主題的結尾。  
  
 計算機合約已修改成包含 <xref:System.ServiceModel.FaultContractAttribute>，如下列範例程式碼所示。 此外，<xref:System.ServiceModel.XmlSerializerFormatAttribute> 是用來以 <xref:System.Xml.Serialization.XmlSerializer> 啟用序列化 (Serialization)。 此屬性 (Attribute) 上的 <xref:System.ServiceModel.XmlSerializerFormatAttribute.SupportFaults%2A> 屬性 (Property) 設為 `true`，指示序列化程式使用 <xref:System.Xml.Serialization.XmlSerializer> 來讀取和寫入錯誤。  
  
```csharp
[XmlSerializerFormat(SupportFaults=true)]  
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface ICalculator  
{  
    [OperationContract]  
    int Add(int n1, int n2);  
  
    [OperationContract]  
    int Subtract(int n1, int n2);  
  
    [OperationContract]  
    int Multiply(int n1, int n2);  
  
    [OperationContract]  
    [FaultContract(typeof(MathFault))]  
    int Divide(int n1, int n2);  
}  
```  
  
 產生用戶端 proxy 的程式碼時, 您必須將 **/UseSerializerForFaults**旗標套用至[System.servicemodel 中繼資料公用程式工具 (Svcutil)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>若要安裝、建置及執行範例  
  
1. 請確定您已[針對 Windows Communication Foundation 範例執行一次安裝程式](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要建置方案的 C# 或 Visual Basic .NET 版本，請遵循 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)中的指示。  
  
3. 若要在單一或跨電腦設定中執行範例, 請遵循執行[Windows Communication Foundation 範例](../../../../docs/framework/wcf/samples/running-the-samples.md)中的指示。  
  
> [!IMPORTANT]
> 這些範例可能已安裝在您的電腦上。 請先檢查下列 (預設) 目錄，然後再繼續。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> 如果此目錄不存在, 請移至[.NET Framework 4 的 Windows Communication Foundation (wcf) 和 Windows Workflow Foundation (WF) 範例](https://go.microsoft.com/fwlink/?LinkId=150780), 以下載所有 Windows Communication Foundation (wcf) [!INCLUDE[wf1](../../../../includes/wf1-md.md)]和範例。 此範例位於下列目錄。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Service\XmlSerializerFaults`  
  
## <a name="see-also"></a>另請參閱

- <xref:System.ServiceModel.XmlSerializerFormatAttribute>
- <xref:System.ServiceModel.XmlSerializerFormatAttribute.SupportFaults%2A>
