---
title: -deterministic (C# 編譯器選項)
ms.date: 04/12/2018
f1_keywords:
- /deterministic
helpviewer_keywords:
- -deterministic compiler option [C#]
- deterministic compiler option [C#]
- /deterministic compiler option [C#]
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: e77c14a1c3a4ba11b8ae6556be4f1c3c0cd42788
ms.sourcegitcommit: 2d792961ed48f235cf413d6031576373c3050918
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2019
ms.locfileid: "70202916"
---
# <a name="-deterministic"></a>-deterministic

可讓編譯器產生相同輸入之編譯間的逐一位元組輸出相同的組件。

## <a name="syntax"></a>語法

```console
-deterministic
```

## <a name="remarks"></a>備註

根據預設，一組指定輸入的編譯器輸出是唯一的，因為編譯器會新增時間戳記以及透過亂數所產生的 GUID。 您可以使用 `-deterministic` 選項來產生「確定性組件」  ，這是只要輸入維持不變，其二進位內容在編譯之間就相同的組件。

編譯器會基於確定性而考慮下列輸入：

- 命令列參數序列。
- 編譯器之 .rsp 回應檔的內容。
- 使用的編譯器精確版本和其參考的組件。
- 目前的目錄路徑。
- 以直接或間接方式明確地傳遞給編譯器之所有檔案的二進位內容，包含：
  - 原始程式檔
  - 參考的組件
  - 參考的模組
  - 資源
  - 強式名稱金鑰檔
  - @ 回應檔
  - 分析器
  - 規則集
  - 分析器可能使用的其他檔案
- 目前文化特性 (Culture) (適用於用來產生診斷和例外狀況訊息的語言)。
- 如果未指定編碼，則為預設編碼 (或目前字碼頁)。
- 存在、不存在，以及編譯器搜尋路徑 (例如，透過 `/lib` 或 `/recurse` 指定) 上檔案的內容。
- 在其上執行編譯器的 CLR 平台。
- `%LIBPATH%` 的值，可能會影響分析器相依性載入。

來源公開可用時，確定性編譯可以用於建立是否從信任的來源編譯二進位檔。 它也可以用於持續建置系統，確定是否需要執行與二進位檔變更相依的建置步驟。

## <a name="see-also"></a>另請參閱

- [C# 編譯器選項](./index.md)
- [管理專案和方案屬性](/visualstudio/ide/managing-project-and-solution-properties)
