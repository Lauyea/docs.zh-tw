---
title: CorFlags.exe (CorFlags 轉換工具)
ms.date: 03/30/2017
helpviewer_keywords:
- CorFlags conversion tool
- CorFlags.exe
- portable executable files, CorFlags section
ms.assetid: ef900f8f-71ca-4dde-9b8c-95ddb0d7d89c
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: b420fb451bf1bb2078a4419a648a1407c39ad178
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2019
ms.locfileid: "71044746"
---
# <a name="corflagsexe-corflags-conversion-tool"></a>CorFlags.exe (CorFlags 轉換工具)
CorFlags 轉換工具可讓您設定可攜式執行映像標頭的 CorFlags 區段。  
  
 此工具會自動與 Visual Studio 一起安裝。 若要執行這項工具，請使用 [Visual Studio 開發人員命令提示字元] (或 Windows 7 中的 [Visual Studio 命令提示字元])。 如需詳細資訊，請參閱[命令提示字元](developer-command-prompt-for-vs.md)。  
  
 在命令提示字元下輸入下列命令：  
  
## <a name="syntax"></a>語法  
  
```console  
CorFlags.exe assembly [options]  
```  
  
## <a name="parameters"></a>參數  
  
|必要參數|說明|  
|------------------------|-----------------|  
|`assembly`|要設定其 CorFlags 的組件名稱。|  
  
|選項|描述|  
|------------|-----------------|  
|**/32BIT[REQ]+**|設定 32BITREQUIRED 旗標。|  
|**/32BIT[REQ]-**|清除 32BITREQUIRED 旗標。|  
|**/32BITPREF+**|設定 32BITPREFERRED 旗標。 應用程式在 64 位元平台上仍然以 32 位元處理序執行。 請只對 EXE 檔案設定這個旗標。 如果在 DLL 上設定這個旗標，DLL 就無法在 64 位元處理序中載入，如此就會擲回 <xref:System.BadImageFormatException> 例外狀況。 具有這個旗標的 EXE 檔案可以載入至 64 位元處理序。<br /><br /> .NET Framework 4.5 中的新增功能。|  
|**/32BITPREF-**|清除 32BITPREFERRED 旗標。<br /><br /> .NET Framework 4.5 中的新增功能。|  
|**/?**|顯示工具的命令語法和選項。|  
|**/Force**|即使組件採用強式名稱，仍然強制進行更新。 **重要：** 如果要更新採用強式名稱的組件，就必須先再次簽署該組件，再執行其程式碼。|  
|**/help**|顯示工具的命令語法和選項。|  
|**/ILONLY+**|設定 ILONLY 旗標。|  
|**/ILONLY-**|清除 ILONLY 旗標。|  
|**/nologo**|隱藏 Microsoft 程式啟始資訊顯示。|  
|**/RevertCLRHeader**|將 CLR 標頭版本還原為 2.0。|  
|**/UpgradeCLRHeader**|將 CLR 標頭版本升級到 2.5。 **注意：** 組件的 CLR 標頭必須是 2.5 (含) 以上版本，才能以原生的形式執行。|  
  
## <a name="remarks"></a>備註  
 如果未指定任何選項，則 CorFlags 轉換工具會針對所指定組件顯示旗標。  
  
## <a name="see-also"></a>另請參閱

- [工具](index.md)
- [64 位元應用程式](../64-bit-apps.md)
- [命令提示字元](developer-command-prompt-for-vs.md)
