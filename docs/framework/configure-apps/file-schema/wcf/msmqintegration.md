---
title: <msmqIntegration>
ms.date: 03/30/2017
ms.assetid: ab677405-1ffe-457a-803f-00c1770e51e2
ms.openlocfilehash: 81ddd8b61327a95db4f040cc2c68cfb52d7d8d2b
ms.sourcegitcommit: 093571de904fc7979e85ef3c048547d0accb1d8a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70400170"
---
# <a name="msmqintegration"></a>\<msmqIntegration>
指定自訂繫結的 MSMQ 傳輸。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<System.servicemodel >** ](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<系結 >** ](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<customBinding >** ](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<系結 >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<Msmqintegration 繫結項目 >**  
  
## <a name="syntax"></a>語法  
  
```xml  
<msmqIntegration customDeadLetterQueue="Uri"
                 deadLetterQueue="Custom/None/System"
                 durable="Boolean"
                 exactlyOnce="Boolean"
                 manualAddressing="Boolean"
                 maxBufferPoolSize="Integer"
                 maxImmediateRetries="Integer"
                 maxReceivedMessageSize="Integer"
                 maxRetryCycles="Integer"
                 rejectAfterLastRetry="Boolean"
                 retryCycleDelay="TimeSpan"
                 serializationFormat="XML/Binary/ActiveX/ByteArray/Stream"
                 timeToLive="TimeSpan"
                 useSourceJournal="Boolean"
                 useMsmqTracing="Boolean">
  <msmqTransportSecurity>
  </msmqTransportSecurity>
</msmqIntegration>
```  
  
## <a name="type"></a>類型  
 `Type`  
  
## <a name="attributes-and-elements"></a>屬性和項目  
 下列各節描述屬性、子項目和父項目。  
  
### <a name="attributes"></a>屬性  
  
|屬性|描述|  
|---------------|-----------------|  
|customDeadLetterQueue|URI，表示每個應用程式寄不出的信件佇列的位置，所有過期或無法傳遞給應用程式的訊息傳輸都會傳輸到這裡。<br /><br /> 如果是需要 ExactlyOnce 保證的訊息 (也就是 `exactlyOnce` 設定為 `true`)，這個屬性預設為 MSMQ 中整個系統的異動式寄不出信件佇列。<br /><br /> 如果是不需要保證的訊息 (也就是 `exactlyOnce` 設為 `false`)，這個屬性預設為 `null`。<br /><br /> 該值必須使用 net.msmq 配置， 預設為 `null`。<br /><br /> 如果 `deadLetterQueue` 設為 `None` 或 `System`，則此屬性必須設為 `null`。 如果這個屬性不是 `null`，則 `deadLetterQueue` 必須設為 `Custom`。|  
|deadLetterQueue|指定要使用的寄不出的信件佇列類型。<br /><br /> 有效值包括：<br /><br /> 客戶自訂 deadletter 佇列。<br />無不會使用 deadletter 佇列。<br />筆記本電腦使用系統 deadletter 佇列。<br /><br /> 此屬性的型別為 DeadLetterQueue。|  
|durable|布林值，這個值會指定由這個繫結處理的訊息是否具有永久性或變動性。 預設為 `true`。<br /><br /> 永久性的訊息不受佇列管理員毀損的影響，但變動性訊息會受到影響。 如果應用程式需要較短的延遲時間，並可以容許訊息偶爾遺失，變動性訊息就很有用。<br /><br /> 如果 `exactlyOnce` 設定為 `true`，訊息必須是永久性的。|  
|exactlyOnce|布林值，這個值會指定是否只要接收一次由此繫結處理的訊息。 預設為 `true`。<br /><br /> 訊息可以在有保證或無保證的情況下傳送。 有了保證，就可以使應用程式確保傳送的訊息已到達接收訊息佇列，如果沒有到達接收訊息佇列，那麼應用程式可以讀取寄不出的信件佇列來判斷這件事。<br /><br /> 當設定為 `exactlyOnce` 時，`true` 表示 MSMQ 將確保傳送的訊息只會傳遞到接收訊息佇列一次，如果傳遞失敗，訊息就會傳送到寄不出的信件佇列。<br /><br /> `exactlyOnce` 設定為 `true` 的已傳送訊息，必須只能傳送到交易式佇列。|  
|manualAddressing|布林值，讓使用者能夠控制訊息定址。 這個屬性通常用於路由器案例，其中應用程式會決定要將訊息傳送到其中一個目的端。<br /><br /> 設定為 `true` 時，通道會假設訊息已經定址，並且不會加入任何其他的資訊。 接著使用者可個別定址每一個訊息。<br /><br /> 設定為 `false` 時，預設的 Windows Communication Foundation (WCF) 定址機制會自動為所有訊息建立位址。<br /><br /> 預設為 `false`。|  
|maxBufferPoolSize|正整數，指定緩衝集區的大小上限。 預設值為 524288。<br /><br /> WCF 有許多組件會使用緩衝區。 每次使用這些組件時建立並終結緩衝區是高度耗費資源的作業，回收緩衝區的記憶體也是如此。 有了緩衝集區，您就可以從集區取出緩衝區來使用，用完後再還給集區， 因此可以避免建立及終結緩衝區的負荷。|  
|maxImmediateRetries|整數，指定從應用程式佇列讀取之訊息的立即重試次數上限。 預設值為 5。<br /><br /> 如果達到訊息立即重試的次數上限，且應用程式未使用訊息，訊息便會傳送到重試佇列，以便日後再次重試。 如果沒有指定重試循環，訊息就會傳送至有害訊息佇列，或者負認可會傳回給寄件者。|  
|maxReceivedMessageSize|正整數，指定包含標頭之訊息的大小上限 (以位元組為單位)。 當對收件者而言訊息太大時，寄件者便會收到 SOAP 錯誤。 收件者會捨棄訊息，然後在追蹤記錄檔中建立此事件的項目。 預設值為 65536。|  
|maxRetryCycles|整數，指定嘗試傳遞訊息至接收應用程式的重試循環次數上限。 預設為 <xref:System.Int32.MaxValue>。<br /><br /> 單一重試循環會嘗試以指定的次數，傳遞訊息至應用程式。 嘗試的次數是由 `maxImmediateRetries` 屬性所設定。 如果傳遞嘗試次數用完以後，應用程式無法使用訊息，訊息就會傳送至重試佇列。 後續的重試循環包含由重試佇列傳回應用程式佇列的訊息，以便於 `retryCycleDelay`屬性指定的延遲之後，再次嘗試傳遞至應用程式。 `maxRetryCycles` 屬性會指定應用程式用來嘗試傳遞訊息的重試循環次數。|  
|rejectAfterLastRetry|布林值，指定達到重試次數上限之後，對傳遞失敗的訊息所要採取的動作。<br /><br /> `true` 表示將負認可傳回寄件者，並將訊息捨棄；`false` 表示將訊息傳送至有害訊息佇列。 預設為 `false`。<br /><br /> 若該值為 `false`，則接收應用程式可讀取有害訊息佇列以處理有害訊息 (也就是傳遞失敗的訊息)。<br /><br /> 因為 MSMQ 3.0 不支援將負認可傳回寄件者，所以 MSMQ 3.0 中會忽略此屬性。|  
|retryCycleDelay|<xref:System.TimeSpan>，指定嘗試傳遞無法立即傳遞之訊息時，重試循環之間的時間延遲。 預設為 00:10:00。<br /><br /> 單一重試循環會嘗試以指定的次數，傳遞訊息至接收應用程式。 嘗試的次數是由 `maxImmediateRetries` 屬性指定。 在立即重試達指定次數後，如果應用程式無法使用訊息，訊息就會傳送至重試佇列。 後續的重試循環包含由重試佇列傳回應用程式佇列的訊息，以便於 `retryCycleDelay`屬性指定的延遲之後，再次嘗試傳遞至應用程式。 重試循環的次數是由 `maxRetryCycles` 屬性所指定。|  
|serializationFormat|指定用來序列化物件的格式器，這些物件會當做部分 MSMQ 訊息傳送。 有效值為<br /><br /> ActiveX序列化 COM 物件時，會使用 ActiveX 格式器。<br />二將物件序列化為二進位封包。<br />ByteArray將物件序列化為位元組陣列。<br />資料流程將物件序列化為資料流。<br />Stl將物件序列化為 XML 封包。 預設為 XML。<br /><br /> 此屬性的型別為 <xref:System.ServiceModel.MsmqIntegration.MsmqMessageSerializationFormat>。|  
|timeToLive|<xref:System.TimeSpan>，指定訊息在到期並置入寄不出的信件佇列之前，訊息仍保持有效的時間。 預設為一天 (1.00:00:00)。<br /><br /> 設定這個屬性可確保有時效的訊息在由接收應用程式處理之前不會變成過時。 佇列中未由接收應用程式於指定的時間間隔內使用的訊息，即稱為過期訊息。 過期訊息會傳送至特別的佇列，稱為寄不出的信件佇列。 寄不出的信件佇列的位置是使用 `customDeadLetterQueue` 屬性設定，或根據保證設定為適當的預設值。|  
|useMsmqTracing|布林值，指定是否應追蹤由此繫結處理的訊息。 預設為 `false`。<br /><br /> 如果啟用追蹤，每當訊息離開或到達訊息佇列電腦時，都會建立報告訊息並傳送至報告佇列。|  
|useSourceJournal|布林值，指定由此繫結所處理之訊息複本是否應該儲存在來源日誌佇列。 預設為 `false`。<br /><br /> 有些佇列應用程式要記錄已離開電腦輸出佇列的訊息，這些程式可以將訊息複製到日誌佇列。 只要訊息一離開輸出佇列，而且收到目的端電腦已收到訊息的認可，訊息的複本就會保留在傳送端電腦的系統日誌佇列中。|  
  
### <a name="child-elements"></a>子元素  
  
|項目|說明|  
|-------------|-----------------|  
|msmqTransportSecurity|指定此繫結的傳輸安全性設定。 此項目的型別為 <xref:System.ServiceModel.Configuration.MsmqTransportSecurityElement>。|  
  
### <a name="parent-elements"></a>父項目  
  
|項目|描述|  
|-------------|-----------------|  
|[\<binding>](../../../misc/binding.md)|定義自訂繫結的所有繫結功能。|  
  
## <a name="see-also"></a>另請參閱

- <xref:System.ServiceModel.Configuration.MsmqIntegrationElement>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [傳輸](../../../wcf/feature-details/transports.md)
- [WCF 中的佇列](../../../wcf/feature-details/queues-in-wcf.md)
- [選擇傳輸](../../../wcf/feature-details/choosing-a-transport.md)
- [繫結](../../../wcf/bindings.md)
- [擴充繫結](../../../wcf/extending/extending-bindings.md)
- [自訂繫結](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
