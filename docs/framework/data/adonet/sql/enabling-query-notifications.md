---
title: 啟用查詢通知
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a5333e19-8e55-4aa9-82dc-ca8745e516ed
ms.openlocfilehash: 9919bad113eb11a38ce137a2cbbf6c67bd5b21ef
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70794081"
---
# <a name="enabling-query-notifications"></a>啟用查詢通知
使用查詢通知的應用程式具有通用要求集。 您必須正確設定資料來源才能支援 SQL 查詢通知，而且使用者必須具有正確的用戶端及伺服器端權限。  
  
 若要使用查詢通知，您必須：  
  
- 啟用資料庫的查詢通知。  
  
- 確保用於連接至資料庫的使用者 ID 具有必要的使用權限。  
  
- 將可執行有效 SELECT 陳述式的 <xref:System.Data.SqlClient.SqlCommand> 物件，與關聯的通知物件 (<xref:System.Data.SqlClient.SqlDependency> 或 <xref:System.Data.Sql.SqlNotificationRequest>) 搭配使用。  
  
- 受監視的資料變更時，提供處理通知的程式碼。  
  
## <a name="query-notifications-requirements"></a>查詢通知需求  
 查詢通知僅支援符合特定需求清單的 SELECT 陳述式。 下表將提供《SQL Server 線上叢書》中 Service Broker 和查詢通知文件的連結。  
  
 **SQL Server 檔**  
  
- [建立通知的查詢](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms181122(v=sql.105))  
  
- [Service Broker 的安全性考慮](https://docs.microsoft.com/previous-versions/sql/sql-server-2005/ms166059(v=sql.90))  
  
- [安全性與保護（Service Broker）](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/bb522911(v=sql.105))  
  
- [通知服務的安全性考慮](https://docs.microsoft.com/previous-versions/sql/sql-server-2005/ms172604(v=sql.90))  
  
- [查詢通知許可權](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms188311(v=sql.105))  
  
- [Service Broker 的國際考慮](https://docs.microsoft.com/previous-versions/sql/sql-server-2005/ms166028(v=sql.90))  
  
- [解決方案設計考慮（Service Broker）](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/bb522899(v=sql.105))  
  
- [Service Broker 開發人員資訊中心](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms166100(v=sql.105))  
  
- [開發人員指南（Service Broker）](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/bb522908(v=sql.105))  
  
## <a name="enabling-query-notifications-to-run-sample-code"></a>啟用查詢通知來執行範例程式碼  
 若要使用 SQL Server Management Studio 在**AdventureWorks**資料庫上啟用 Service Broker，請執行下列 transact-sql 語句：  
  
 `ALTER DATABASE AdventureWorks SET ENABLE_BROKER;`  
  
 若要讓查詢通知範例正常執行，您必須在資料庫伺服器上執行下列 Transact-SQL 陳述式。  
  
```  
CREATE QUEUE ContactChangeMessages;  
  
CREATE SERVICE ContactChangeNotifications  
  ON QUEUE ContactChangeMessages  
([http://schemas.microsoft.com/SQL/Notifications/PostQueryNotification]);  
```  
  
## <a name="query-notifications-permissions"></a>查詢通知使用權限  
 執行要求通知之命令的使用者，必須在伺服器上具有 SUBSCRIBE QUERY NOTIFICATIONS 資料庫的使用權限。  
  
 在部分信任狀況下執行的用戶端程式碼要求 <xref:System.Data.SqlClient.SqlClientPermission>。  
  
 下列程式碼會建立 <xref:System.Data.SqlClient.SqlClientPermission> 物件，並將 <xref:System.Security.Permissions.PermissionState> 設定為 <xref:System.Security.Permissions.PermissionState.Unrestricted>。 如果在呼叫堆疊中較高的所有呼叫端都尚未被授與此權限，<xref:System.Security.CodeAccessPermission.Demand%2A> 將在執行階段強制執行 <xref:System.Security.SecurityException>。  
  
 [!code-csharp[DataWorks SqlNotification.Perms#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlNotification.Perms/CS/source.cs#1)]
 [!code-vb[DataWorks SqlNotification.Perms#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlNotification.Perms/VB/source.vb#1)]  
  
## <a name="choosing-a-notification-object"></a>選擇通知物件  
 查詢通知 API 提供兩個物件來處理通知：<xref:System.Data.SqlClient.SqlDependency> 與 <xref:System.Data.Sql.SqlNotificationRequest>。 一般而言，大多數非 ASP.NET 應用程式應使用 <xref:System.Data.SqlClient.SqlDependency> 物件。 ASP.NET 應用程式應使用較高層級的 <xref:System.Web.Caching.SqlCacheDependency>，其包裝了 <xref:System.Data.SqlClient.SqlDependency>，並提供用於管理通知及快取物件的架構。  
  
### <a name="using-sqldependency"></a>使用 SqlDependency  
 若要使用 <xref:System.Data.SqlClient.SqlDependency>，所使用的 SQL Server 資料庫必須啟用 Service Broker，而且使用者必須有接收通知的使用權限。 Service Broker 物件 (如通知佇列) 是預先定義的。  
  
 此外，<xref:System.Data.SqlClient.SqlDependency> 會自動啟動工作執行緒，以便在公佈到佇列時處理通知；它也會剖析 Service Broker 訊息，將資訊做為事件引數資料公開。 <xref:System.Data.SqlClient.SqlDependency> 必須藉由呼叫 `Start` 方法來初始化，以便建立對資料庫的相依性。 這是一種靜態方法，您僅需要在所要求之每個資料庫連接的應用程式初始化期間內，呼叫一次。 對於每個進行的相依性連接，`Stop` 方法應該在應用程式終止時呼叫。  
  
### <a name="using-sqlnotificationrequest"></a>使用 SqlNotificationRequest  
 相反，<xref:System.Data.Sql.SqlNotificationRequest> 要求您自己實作整個接聽基礎結構。 此外，必須定義所有支援的 Service Broker 物件，如佇列、服務及佇列所支援的訊息類型。 如果應用程式要求特別通知訊息或通知行為，或者應用程式為較大 Service Broker 應用程式的一部分，則此手動方法非常有用。  
  
## <a name="see-also"></a>另請參閱

- [SQL Server 中的查詢通知](query-notifications-in-sql-server.md)
- [ADO.NET 概觀](../ado-net-overview.md)
