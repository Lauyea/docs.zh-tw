---
title: HOW TO：啟用工作流程與工作流程服務的持續性
ms.date: 03/30/2017
ms.assetid: 2b1c8bf3-9866-45a4-b06d-ee562393e503
ms.openlocfilehash: 9357098318342d15ad7eead32cbc7218af095f6e
ms.sourcegitcommit: 9b1ac36b6c80176fd4e20eb5bfcbd9d56c3264cf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2019
ms.locfileid: "67425343"
---
# <a name="how-to-enable-persistence-for-workflows-and-workflow-services"></a>HOW TO：啟用工作流程與工作流程服務的持續性

本主題描述如何啟用工作流程與工作流程服務的持續性。

## <a name="enable-persistence-for-workflows"></a>啟用工作流程的持續性

您可以建立與執行個體存放區的關聯**WorkflowApplication**利用<xref:System.Activities.WorkflowApplication.InstanceStore%2A>屬性<xref:System.Activities.WorkflowApplication>類別。 <xref:System.Activities.WorkflowApplication.Persist%2A> 方法會將工作流程儲存或保存在與應用程式相關的執行個體存放區中。 <xref:System.Activities.WorkflowApplication.Unload%2A> 方法會將工作流程保存在執行個體存放區中，然後從記憶體卸載該執行個體。 **負載**方法會載入使用執行個體持續性存放區中的工作流程資料的記憶體中的工作流程。

**Persist**方法會執行下列步驟：

1. 暫停工作流程排程器，直到工作流程進入閒置狀態為止。

2. 將工作流程保存或儲存在持續性存放區中。

3. 恢復工作流程排程器。

 **卸載**方法會執行下列步驟：

1. 暫停工作流程排程器，直到工作流程進入閒置狀態為止。

2. 將工作流程保存或儲存在持續性存放區中。

3. 處置記憶體中的工作流程執行個體。

這兩個**Persist**並**卸載**方法將會封鎖直到工作流程離開不保存區域工作流程是在非保存區域時。 不保存區完成後，此方法會繼續進行保存或卸載作業。 如果不保存區未在逾時時間過去前完成，或者持續性處理序所花的時間過長，就會擲回 TimeoutException。

## <a name="enable-persistence-for-workflow-services-in-code"></a>在程式碼中啟用工作流程服務的持續性

**DurableInstancingOptions**隸屬<xref:System.ServiceModel.WorkflowServiceHost>類別具有名為**InstanceStore**可用來建立與執行個體存放區的關聯**WorkflowServiceHost**.

```csharp
// wsh is an instance of WorkflowServiceHost class
wsh.DurableInstancingOptions.InstanceStore = new SqlWorkflowInstanceStore();
```

當**WorkflowServiceHost**已開啟，持續性會自動啟用**DurableInstancingOptions.InstanceStore**不是 null。

一般而言，服務行為會提供搭配使用工作流程服務主機的實體執行個體存放區**InstanceStore**屬性。 例如，SqlWorkflowInstanceStoreBehavior 會建立的執行個體**SqlWorkflowInstanceStore**、 加以設定，然後將其指派給**DurableInstancingOptions.InstanceStore**。

## <a name="enable-persistence-for-workflow-services-using-an-application-configuration-file"></a>使用應用程式組態檔啟用工作流程服務的持續性

在 app.config 或 web.config 檔案中加入下列程式碼，即可使用應用程式組態檔啟用持續性：

```xml
<configuration>
  <system.serviceModel>
    <behaviors>
      <serviceBehaviors>
        <behavior name="myBehavior">
          <SqlWorkflowInstanceStore connectionString="Data Source=myDatabaseServer;Initial Catalog=myPersistenceDatabase">
        </behavior>
      </serviceBehaviors>
    <behaviors>
  </system.serviceModel>
</configuration>
```
