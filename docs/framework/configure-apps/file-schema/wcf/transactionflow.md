---
title: <transactionFlow>
ms.date: 03/30/2017
ms.assetid: 8c7b4c5b-ace3-4fe3-89ff-7b13c9aacd13
ms.openlocfilehash: 8247dd62f4dda853487fe52f00f8f548b627c075
ms.sourcegitcommit: 093571de904fc7979e85ef3c048547d0accb1d8a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70399402"
---
# <a name="transactionflow"></a>\<transactionFlow>
指定自訂繫結的交易流程支援。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<System.servicemodel >** ](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<系結 >** ](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<customBinding >** ](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<系結 >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<transactionFlow >**  
  
## <a name="syntax"></a>語法  
  
```xml  
<transactionFlow transactionProtocol="OleTransactions/WSAtomicTransactionOctober2004" />
```  
  
## <a name="attributes-and-elements"></a>屬性和項目  
 下列各節描述屬性、子項目和父項目。  
  
### <a name="attributes"></a>屬性  
  
|屬性|說明|  
|---------------|-----------------|  
|transactionProtocol|指定要使用的異動通訊協定。 有效值包括以下的值：<br /><br /> -OleTransactions<br />-   WSAtomicTransactionOctober2004<br /><br /> 預設值為 OleTransactions。<br /><br /> 此屬性的型別為 <xref:System.ServiceModel.TransactionProtocol>。|  
  
### <a name="child-elements"></a>子元素  
 無。  
  
### <a name="parent-elements"></a>父項目  
  
|項目|說明|  
|-------------|-----------------|  
|[\<binding>](../../../misc/binding.md)|定義自訂繫結的所有繫結功能。|  
  
## <a name="remarks"></a>備註  
 這個項目可以讓您啟用或停用端點繫結設定的傳入交易流程，以及指定傳入交易的所需通訊協定格式。 如需使用此設定元素的詳細資訊，請參閱[配置交易](../../../wcf/feature-details/servicemodel-transaction-configuration.md)設定和[啟用交易流程](../../../wcf/feature-details/enabling-transaction-flow.md)。  
  
> [!CAUTION]
> 當使用 `OleTransactions` 通訊協定在端點之間流動交易時，如果目的端點嘗試使用任何 `OleTransactions` 以外的通訊協定再次流動時，交易逾時可能會遺失。 這會導致 OleTransactions 躍點後的所有下層節點比預期的時間更晚發生逾時。  
  
## <a name="see-also"></a>另請參閱

- <xref:System.ServiceModel.Configuration.TransactionFlowElement>
- <xref:System.ServiceModel.Channels.TransactionFlowBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [ServiceModel 異動組態](../../../wcf/feature-details/servicemodel-transaction-configuration.md)
- [啟用異動流程](../../../wcf/feature-details/enabling-transaction-flow.md)
- [繫結](../../../wcf/bindings.md)
- [擴充繫結](../../../wcf/extending/extending-bindings.md)
- [自訂繫結](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
