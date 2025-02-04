---
title: -平台 (Visual Basic)
ms.date: 03/13/2018
helpviewer_keywords:
- platform compiler option [Visual Basic]
- /platform compiler option [Visual Basic]
- -platform compiler option [Visual Basic]
ms.assetid: f9bc61e6-e854-4ae1-87b9-d6244de23fd1
ms.openlocfilehash: eb5513c6d8e4085e1b3f69de1d36a007fa27271e
ms.sourcegitcommit: 4735bb7741555bcb870d7b42964d3774f4897a6e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/30/2019
ms.locfileid: "66380305"
---
# <a name="-platform-visual-basic"></a>-平台 (Visual Basic)
指定通用語言執行平台 (CLR) 的哪個平台版本可以執行輸出檔。  
  
## <a name="syntax"></a>語法  
  
```  
-platform:{ x86 | x64 | Itanium | arm | anycpu | anycpu32bitpreferred }  
```  
  
## <a name="arguments"></a>引數  
  
|詞彙|定義|  
|---|---|  
|`x86`|將組件編譯為以 32 位元、與 x86 相容的 CLR 執行。|  
|`x64`|在支援 AMD64 或 EM64T 指令集的電腦上編譯將由 64 位元 CLR 所執行的組件。|  
|`Itanium`|將組件編譯為可以在使用 Itanium 處理器的電腦上以 64 位元 CLR 執行。|  
|`arm`|將組件編譯為可以在配備 ARM (進階 RISC 機器) 處理器的電腦上執行。|  
|`anycpu`|編譯要在任何平台上執行的組件。 應用程式將在 32 位元版本的 Windows 上當做 32 位元應用程式執行，並在 64 位元版本的 Windows 上當做 64 位元應用程式執行。 此旗標為預設值。|  
|`anycpu32bitpreferred`|編譯要在任何平台上執行的組件。 應用程式將在 32 位元和 64 位元版本的 Windows 上當做 32 位元應用程式執行。 這個旗標是僅適用於可執行檔 (。EXE)，而且需要.NET Framework 4.5。|  
  
## <a name="remarks"></a>備註  
 使用 `-platform` 選項指定做為輸出檔目標的處理器類型。  
  
 一般而言，不論是何種平台，以 Visual Basic 撰寫的.NET Framework 組件將會執行相同作業。 不過，有某些情況下，在不同的平台上有不同的行為。 常見的情況如下：  
  
- 包含的成員視平台而變更大小的結構，例如任何指標類型。  
  
- 包含常數大小的指標算術。  
  
- 不正確的平台叫用，或是使用 `Integer` 而不是 <xref:System.IntPtr> 作為控點的 COM 宣告。  
  
- 將 <xref:System.IntPtr> 轉型為 `Integer`。  
  
- 使用其元件不存在於所有平台的平台叫用或 COM Interop。  
  
 **-平台**選項可以減少一些問題，如果您知道您所做的假設會執行您的程式碼的架構。 尤其是：  
  
- 如果您決定以 64 位元平台為目標，並在 32 位元電腦上執行應用程式，錯誤訊息會遠遠較早出現，而且會較以平台為目標，超過以不使用這個參數所發生的錯誤為目標。  
  
- 如果您在選項上設定 `x86` 旗標，且隨後在 64 位元電腦上執行應用程式，則應用程式會在 WOW 子系統中執行，而不是以原生方式執行。  
  
 在 64 位元 Windows 作業系統上：  
  
- 使用 `-platform:x86` 編譯的組件將在 WOW64 下執行的 32 位元 CLR 上執行。  
  
- 使用 `-platform:anycpu` 編譯的可執行檔將在 64 位元 CLR 上執行。  
  
- 使用 `-platform:anycpu` 編譯的 DLL 將在與其載入所在處理序相同的 CLR 上執行。  
  
- 使用 `-platform:anycpu32bitpreferred` 編譯的可執行檔將在 32 位元 CLR 上執行。  
  
 如需如何開發應用程式在 64 位元版本的 Windows 上執行的詳細資訊，請參閱[64 位元應用程式](../../../framework/64-bit-apps.md)。  
  
### <a name="to-set--platform-in-the-visual-studio-ide"></a>若要設定-Visual Studio IDE 中的平台  
  
1. 在 **方案總管**，選擇專案，開啟**專案**功能表，然後再按一下**屬性**。  
  
2. 在上**編譯**索引標籤上，選取或清除**偏好 32 位元**核取方塊，或者，在**目標 CPU**清單中，選擇一個值。  
  
     如需詳細資訊，請參閱 <<c0> [ 編譯的 Page，Project Designer (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)。  
  
## <a name="example"></a>範例  
 下列範例說明如何使用 `-platform` 編譯器選項。  
  
```console
vbc -platform:x86 myFile.vb  
```  
  
## <a name="see-also"></a>另請參閱

- [/target (Visual Basic)](target.md)
- [Visual Basic 命令列編譯器](index.md)
- [編譯命令列範例](sample-compilation-command-lines.md)
