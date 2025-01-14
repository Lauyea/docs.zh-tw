---
title: 常值
description: 深入了解中的常值型別F#程式設計語言。
ms.date: 06/28/2019
ms.openlocfilehash: 0c9ced0b505817a161ca39c6c9f853f94cedf410
ms.sourcegitcommit: eaa6d5cd0f4e7189dbe0bd756e9f53508b01989e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2019
ms.locfileid: "67610147"
---
# <a name="literals"></a>常值

> [!NOTE]
> 這篇文章中的 API 參考連結將帶您前往 MSDN （適用於目前）。

本主題提供示範如何指定類型的常值中的資料表F#。

## <a name="literal-types"></a>常值類型

下表顯示中的常值型別F#。 表示之數字的十六進位表示法的字元不區分大小寫，識別類型的字元會區分大小寫。

|類型|描述|後置字元或前置詞|範例|
|----|-----------|----------------|--------|
|sbyte|帶正負號的 8 位元整數|y|`86y`<br /><br />`0b00000101y`|
|byte|不帶正負號的 8 位元自然數|uy|`86uy`<br /><br />`0b00000101uy`|
|int16|帶正負號的 16 位元整數|秒|`86s`|
|uint16|不帶正負號的 16 位元自然數|us|`86us`|
|int<br /><br />int32|帶正負號的 32 位元整數|l 或 none|`86`<br /><br />`86l`|
|uint<br /><br />uint32|不帶正負號的 32 位元自然數|u 或 ul|`86u`<br /><br />`86ul`|
|nativeint|帶正負號自然數的原生指標|n|`123n`|
|unativeint|為不帶正負號自然數的原生指標|取消|`0x00002D3Fun`|
|int64|帶正負號的 64 位元整數|L|`86L`|
|uint64|不帶正負號的 64 位元自然數|UL|`86UL`|
|single、float32|32 位元浮點數|F 或 f|`4.14F` 或 `4.14f`|
|||lf|`0x00000000lf`|
|浮點數;double|64 位元浮點數|none|`4.14` 或 `2.3E+32` 或 `2.3e+32`|
|||LF|`0x0000000000000000LF`|
|bigint|不限於 64 位元表示的整數|I|`9999999999999999999999999999I`|
|decimal|以固定點或有理數表示的小數數字|M 或 m|`0.7833M` 或 `0.7833m`|
|Char|Unicode 字元|none|`'a'`|
|String|Unicode 字串|none|`"text\n"`<br /><br />或<br /><br />`@"c:\filename"`<br /><br />或<br /><br />`"""<book title="Paradise Lost">"""`<br /><br />或<br /><br />`"string1" + "string2"`<br /><br />另請參閱[字串](Strings.md)。|
|byte|ASCII 字元|B|`'a'B`|
|byte[]|ASCII 字串|B|`"text"B`|
|字串或 byte]|逐字字串|@ prefix|`@"\\server\share"` (Unicode)<br /><br />`@"\\server\share"B` (ASCII)|

## <a name="named-literals"></a>具名常值

預定要當做常數的值都可以使用標記[常值](https://msdn.microsoft.com/library/465f36ce-d146-41c0-b425-679c509cd285)屬性。 此屬性會有效果會使要編譯成常數的值。

在模式比對運算式，開頭為小寫字元的識別碼都被視為變數繫結，而非做為常值，因此您通常應該使用第一個字母大寫定義常值。

```fsharp
[<Literal>]
let SomeJson = """{"numbers":[1,2,3,4,5]}"""

[<Literal>]
let Literal1 = "a" + "b"

[<Literal>]
let FileLocation =   __SOURCE_DIRECTORY__ + "/" + __SOURCE_FILE__

[<Literal>]
let Literal2 = 1 ||| 64

[<Literal>]
let Literal3 = System.IO.FileAccess.Read ||| System.IO.FileAccess.Write
```

## <a name="remarks"></a>備註

Unicode 字串可以包含您可以使用指定的明確編碼`\u`後面的 16 位元十六進位字碼 (0000-FFFF) 或您可以使用指定的 UTF-32 編碼`\U`後面接著 32 位元的十六進位代碼，表示任何 Unicode 字碼指標 (00000000-0010FFFF)。

其他位元運算子，而不使用`|||`不允許。

## <a name="integers-in-other-bases"></a>在其他基底的整數

帶正負號的 32 位元整數也 zadat 中使用十六進位、 八進位或二進位`0x`，`0o`或`0b`分別的前置詞。

```fsharp
let numbers = (0x9F, 0o77, 0b1010)
// Result: numbers : int * int * int = (159, 63, 10)
```

## <a name="underscores-in-numeric-literals"></a>數值常值中的底線

您可以使用底線字元來分隔數字 (`_`)。

```fsharp
let value = 0xDEAD_BEEF

let valueAsBits = 0b1101_1110_1010_1101_1011_1110_1110_1111

let exampleSSN = 123_456_7890
```

## <a name="see-also"></a>另請參閱

- [Core.LiteralAttribute 類別](https://msdn.microsoft.com/visualfsharpdocs/conceptual/core.literalattribute-class-%5bfsharp%5d)
