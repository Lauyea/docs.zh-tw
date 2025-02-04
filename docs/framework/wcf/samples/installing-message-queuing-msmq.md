---
title: 安裝訊息佇列 (MSMQ)
ms.date: 03/30/2017
ms.assetid: 7ddcd497-3e04-427e-bc04-3610ad98b01e
ms.openlocfilehash: 118143f2d434e9f4399c3e9141743fc0254b61ab
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2019
ms.locfileid: "70039617"
---
# <a name="installing-message-queuing-msmq"></a>安裝訊息佇列 (MSMQ)
下列程序示範如何安裝 Message Queuing 4.0 和 Message Queuing 3.0。  
  
> [!NOTE]
> [!INCLUDE[wxp](../../../../includes/wxp-md.md)] 和 [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] 未提供 Message Queuing 4.0。  
  
#### <a name="to-install-message-queuing-40-on-windows-server-2008-or-windows-server-2008-r2"></a>若要在 Windows Server 2008 或 Windows Server 2008 R2 上安裝 Message Queuing 4.0  
  
1. 在伺服器管理員中, 按一下 [**功能**]。  
  
2. 在右側窗格的 [**功能摘要**] 底下, 按一下 [**新增功能**]。  
  
3. 在產生的視窗中, 展開 [**訊息佇列**]。  
  
4. 展開 [**訊息佇列服務**]。  
  
5. 按一下 [**目錄服務整合**] (針對加入網域的電腦), 然後按一下 [ **HTTP 支援**]。  
  
6. 按 **[下一步]** , 然後按一下 [**安裝**]。  
  
#### <a name="to-install-message-queuing-40-on-windows-7-or-windows-vista"></a>若要在 Windows 7 或 Windows Vista 上安裝 Message Queuing 4.0  
  
1. 開啟 [**控制台**]。  
  
2. 按一下 [**程式**], 然後按一下 [**程式和功能**] 下**的 [開啟或關閉 Windows 功能**]。  
  
3. 展開 [Microsoft Message Queue (MSMQ) 伺服器]，展開 [Microsoft Message Queue (MSMQ) 伺服器核心]，然後選取下列訊息佇列功能的核取方塊進行安裝：  
  
    - MSMQ Active Directory 網域服務整合 (針對加入網域的電腦)。  
  
    - MSMQ HTTP 支援。  
  
4. 按一下 [確定 **Deploying Office Solutions**]。  
  
5. 如果系統提示您重新開機電腦, 請按一下 **[確定]** 以完成安裝。  
  
#### <a name="to-install-message-queuing-30-on-windows-xp-and-windows-server-2003"></a>若要在 Windows XP 和 Windows Server 2003 上安裝 Message Queuing 3.0  
  
1. 開啟 [**控制台**]。  
  
2. 按一下 [新增] [**移除程式**], 然後按一下 [**新增 Windows 元件**]。  
  
3. 選取 [訊息佇列], 然後按一下 [**詳細資料**]。  
  
    > [!NOTE]
    > 如果您執行的是 [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)]，請選取 [應用程式伺服器] 來存取訊息佇列。  
  
4. 確定在詳細資料頁面上選取 MSMQ HTTP 支援的選項。  
  
5. 按一下 **[確定]** 以結束 [詳細資料] 頁面, 然後按 **[下一步]** 。 完成安裝。  
  
6. 如果系統提示您重新開機電腦, 請按一下 **[確定]** 以完成安裝。  
  
## <a name="see-also"></a>另請參閱

- [設定指示](../../../../docs/framework/wcf/samples/set-up-instructions.md)
