---
title: -refout (C# 編譯器選項)
ms.date: 08/08/2017
f1_keywords:
- /refout
helpviewer_keywords:
- refout compiler option [C#]
- /refout compiler option [C#]
- -refout compiler option [C#]
ms.openlocfilehash: 97cbf540527d0449387b71bb1d97df95b6a4aba4
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2019
ms.locfileid: "69602511"
---
# <a name="-refout-c-compiler-options"></a>-refout (C# 編譯器選項)

**-refout** 選項指定參考組件應該輸出的檔案路徑。 這在發出 API 中會轉譯成 `metadataPeStream`。 此選項會對應到 MSBuild 的 [ProduceReferenceAssembly](/visualstudio/msbuild/common-msbuild-project-properties) 專案屬性。

## <a name="syntax"></a>語法

```console
-refout:filepath
```

## <a name="arguments"></a>引數

 `filepath` 參考組件的檔案路徑。 它通常應該符合主要組件的路徑。 建議慣例 (由 MSBuild 使用) 是將參考組件放在相對於主要組件的 "ref/" 子資料夾中。

## <a name="remarks"></a>備註

僅中繼資料組件以單一的 `throw null` 主體取代其方法主體，但包含匿名型別以外的所有成員。 使用 `throw null` 主體的原因 (相對於沒有主體)，是這樣 PEVerify 便可執行和傳遞 (因此驗證中繼資料的完整性)。

參考組件包含組件層級的 `ReferenceAssembly` 屬性。 這個屬性可在來源中指定 (然後編譯器就不需要合成它)。 因為這個屬性，執行階段會拒絕載入參考組件以供執行 (但仍可載入僅限反映模式)。 在組件上反映的工具需要確保它們載入的組件為僅限反映，否則就會從執行階段收到類型載入錯誤。

參考組件會進一步移除來自僅中繼資料組件的中繼資料 (私用成員)：

- 參考組件只有在 API 介面中所需項目的參考。 實際的組件可能有與特定實作相關的其他參考。 例如，`class C { private void M() { dynamic d = 1; ... } }` 的參考組件不參考 `dynamic` 所需的任何類型。
- 如果移除它們不會明顯影響編譯的話，則會移除私用函式的成員 (方法、屬性和事件)。 如果沒有任何 <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 屬性，請對內部函式成員執行相同的作業。
- 但所有類型 (包括私用或巢狀類型) 都會保留在參考組件中。 所有屬性都保留 (即使是內部屬性)。
- 所有虛擬方法都保留。 保留明確的介面實作。 保留明確實作的屬性和事件，因為它們的存取子都是虛擬的 (因此保留)。
- 所有結構欄位都保留。 (這是 post-C#-7.1 細分的候選項目)

`-refout` 和 [`-refonly`](refonly-compiler-option.md) 選項互斥。

## <a name="see-also"></a>另請參閱

- [C# 編譯器選項](./index.md)
- [管理專案和方案屬性](/visualstudio/ide/managing-project-and-solution-properties)
