---
title: dotnet new 命令
description: dotnet new 命令會根據指定的範本建立新的 .NET Core 專案。
ms.date: 05/06/2019
ms.openlocfilehash: b61b5fd53f470c30b636026fa19ebfad834d6354
ms.sourcegitcommit: a4b10e1f2a8bb4e8ff902630855474a0c4f1b37a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2019
ms.locfileid: "71117663"
---
# <a name="dotnet-new"></a>dotnet new

[!INCLUDE [topic-appliesto-net-core-all](../../../includes/topic-appliesto-net-core-all.md)]

## <a name="name"></a>名稱

`dotnet new` - 根據指定的範本建立新的專案、組態檔或方案。

## <a name="synopsis"></a>概要

<!-- markdownlint-disable MD025 -->

# <a name="net-core-22tabnetcore22"></a>[.NET Core 2.2](#tab/netcore22)

```dotnetcli
dotnet new <TEMPLATE> [--dry-run] [--force] [-i|--install] [-lang|--language] [-n|--name] [--nuget-source] [-o|--output] [-u|--uninstall] [Template options]
dotnet new <TEMPLATE> [-l|--list] [--type]
dotnet new [-h|--help]
```

# <a name="net-core-21tabnetcore21"></a>[.NET Core 2.1](#tab/netcore21)

```dotnetcli
dotnet new <TEMPLATE> [--force] [-i|--install] [-lang|--language] [-n|--name] [--nuget-source] [-o|--output] [-u|--uninstall] [Template options]
dotnet new <TEMPLATE> [-l|--list] [--type]
dotnet new [-h|--help]
```

# <a name="net-core-20tabnetcore20"></a>[.NET Core 2.0](#tab/netcore20)

```dotnetcli
dotnet new <TEMPLATE> [--force] [-i|--install] [-lang|--language] [-n|--name] [-o|--output] [-u|--uninstall] [Template options]
dotnet new <TEMPLATE> [-l|--list] [--type]
dotnet new [-h|--help]
```

# <a name="net-core-1xtabnetcore1x"></a>[.NET Core 1.x](#tab/netcore1x)

```dotnetcli
dotnet new <TEMPLATE> [-lang|--language] [-n|--name] [-o|--output] [-all|--show-all] [-h|--help] [Template options]
dotnet new <TEMPLATE> [-l|--list]
dotnet new [-all|--show-all]
dotnet new [-h|--help]
```

---

## <a name="description"></a>描述

`dotnet new` 命令提供便利的方式來初始化有效的 .NET Core 專案。

命令會呼叫[範本引擎](https://github.com/dotnet/templating)，以根據指定的範本和選項在磁碟上建立成品。

## <a name="arguments"></a>引數

`TEMPLATE`

要在叫用命令時具現化的範本。 每個範本可能會有您可以傳遞的特定選項。 如需詳細資訊，請參閱[範本選項](#template-options)。

如果 `TEMPLATE` 值與 [範本] 或 [簡短名稱] 欄中的文字不完全相符，則會對這兩欄執行子字串比對作業。

# <a name="net-core-22tabnetcore22"></a>[.NET Core 2.2](#tab/netcore22)

此命令包含預設的範本清單。 使用 `dotnet new -l` 以取得可用範本的清單。 下表顯示隨 .NET Core SDK 2.2.100 預先安裝的範本。 範本的預設語言會顯示在方括號內。

| 範本                                    | 簡短名稱        | 語言     | Tags                                  |
|----------------------------------------------|-------------------|--------------|---------------------------------------|
| 主控台應用程式                          | `console`         | [C#], F#, VB | 通用/主控台                        |
| 類別庫                                | `classlib`        | [C#], F#, VB | 通用/程式庫                        |
| 單元測試專案                            | `mstest`          | [C#], F#, VB | 測試/MSTest                           |
| NUnit 3 測試專案                         | `nunit`           | [C#], F#, VB | 測試/NUnit                            |
| NUnit 3 測試項目                            | `nunit-test`      | [C#], F#, VB | 測試/NUnit                            |
| xUnit 測試專案                           | `xunit`           | [C#], F#, VB | 測試/XUnit                            |
| Razor 頁面                                   | `page`            | [C#]         | Web/ASP.NET                           |
| MVC ViewImports                              | `viewimports`     | [C#]         | Web/ASP.NET                           |
| MVC ViewStart                                | `viewstart`       | [C#]         | Web/ASP.NET                           |
| 空的 ASP.NET Core                           | `web`             | [C#]、F#     | Web/空白                             |
| ASP.NET Core Web 應用程式 (模型檢視控制器) | `mvc`             | [C#]、F#     | Web/MVC                               |
| ASP.NET Core Web 應用程式                         | `webapp`、 `razor` | [C#]         | Web/MVC/Razor 頁面                   |
| ASP.NET Core 與 Angular                    | `angular`         | [C#]         | Web/MVC/SPA                           |
| ASP.NET Core 與 React.js                   | `react`           | [C#]         | Web/MVC/SPA                           |
| ASP.NET Core 與 React.js 和 Redux         | `reactredux`      | [C#]         | Web/MVC/SPA                           |
| Razor 類別庫                          | `razorclasslib`   | [C#]         | Web/Razor/程式庫/Razor 類別庫 |
| ASP.NET Core Web API                         | `webapi`          | [C#]、F#     | Web/WebAPI                            |
| global.json 檔案                             | `globaljson`      |              | 組態                                |
| NuGet 組態                                 | `nugetconfig`     |              | 組態                                |
| Web 組態                                   | `webconfig`       |              | 組態                                |
| 方案檔                                | `sln`             |              | 方案                              |

# <a name="net-core-21tabnetcore21"></a>[.NET Core 2.1](#tab/netcore21)

此命令包含預設的範本清單。 使用 `dotnet new -l` 以取得可用範本的清單。 下表顯示隨 .NET Core SDK 2.1.300 預先安裝的範本。 範本的預設語言會顯示在方括號內。

| 範本                                    | 簡短名稱      | 語言     | Tags                                  |
|----------------------------------------------|-----------------|--------------|---------------------------------------|
| 主控台應用程式                          | `console`       | [C#], F#, VB | 通用/主控台                        |
| 類別庫                                | `classlib`      | [C#], F#, VB | 通用/程式庫                        |
| 單元測試專案                            | `mstest`        | [C#], F#, VB | 測試/MSTest                           |
| xUnit 測試專案                           | `xunit`         | [C#], F#, VB | 測試/XUnit                            |
| Razor 頁面                                   | `page`          | [C#]         | Web/ASP.NET                           |
| MVC ViewImports                              | `viewimports`   | [C#]         | Web/ASP.NET                           |
| MVC ViewStart                                | `viewstart`     | [C#]         | Web/ASP.NET                           |
| 空的 ASP.NET Core                           | `web`           | [C#]、F#     | Web/空白                             |
| ASP.NET Core Web 應用程式 (模型檢視控制器) | `mvc`           | [C#]、F#     | Web/MVC                               |
| ASP.NET Core Web 應用程式                         | `razor`         | [C#]         | Web/MVC/Razor 頁面                   |
| ASP.NET Core 與 Angular                    | `angular`       | [C#]         | Web/MVC/SPA                           |
| ASP.NET Core 與 React.js                   | `react`         | [C#]         | Web/MVC/SPA                           |
| ASP.NET Core 與 React.js 和 Redux         | `reactredux`    | [C#]         | Web/MVC/SPA                           | 
| Razor 類別庫                          | `razorclasslib` | [C#]         | Web/Razor/程式庫/Razor 類別庫 |
| ASP.NET Core Web API                         | `webapi`        | [C#]、F#     | Web/WebAPI                            |
| global.json 檔案                             | `globaljson`    |              | 組態                                |
| NuGet 組態                                 | `nugetconfig`   |              | 組態                                |
| Web 組態                                   | `webconfig`     |              | 組態                                |
| 方案檔                                | `sln`           |              | 方案                              |

# <a name="net-core-20tabnetcore20"></a>[.NET Core 2.0](#tab/netcore20)

此命令包含預設的範本清單。 使用 `dotnet new -l` 以取得可用範本的清單。 下表顯示隨 .NET Core SDK 2.0.0 預先安裝的範本。 範本的預設語言會顯示在方括號內。

| 範本                                    | 簡短名稱    | 語言     | Tags                |
|----------------------------------------------|---------------|--------------|---------------------|
| 主控台應用程式                          | `console`     | [C#], F#, VB | 通用/主控台      |
| 類別庫                                | `classlib`    | [C#], F#, VB | 通用/程式庫      |
| 單元測試專案                            | `mstest`      | [C#], F#, VB | 測試/MSTest         |
| xUnit 測試專案                           | `xunit`       | [C#], F#, VB | 測試/XUnit          |
| 空的 ASP.NET Core                           | `web`         | [C#]、F#     | Web/空白           |
| ASP.NET Core Web 應用程式 (模型檢視控制器) | `mvc`         | [C#]、F#     | Web/MVC             |
| ASP.NET Core Web 應用程式                         | `razor`       | [C#]         | Web/MVC/Razor 頁面 |
| ASP.NET Core 與 Angular                    | `angular`     | [C#]         | Web/MVC/SPA         |
| ASP.NET Core 與 React.js                   | `react`       | [C#]         | Web/MVC/SPA         |
| ASP.NET Core 與 React.js 和 Redux         | `reactredux`  | [C#]         | Web/MVC/SPA         |
| ASP.NET Core Web API                         | `webapi`      | [C#]、F#     | Web/WebAPI          |
| global.json 檔案                             | `globaljson`  |              | 組態              |
| NuGet 組態                                 | `nugetconfig` |              | 組態              |
| Web 組態                                   | `webconfig`   |              | 組態              |
| 方案檔                                | `sln`         |              | 方案            |
| Razor 頁面                                   | `page`        |              | Web/ASP.NET         |
| MVC ViewImports                              | `viewimports` |              | Web/ASP.NET         |
| MVC ViewStart                                | `viewstart`   |              | Web/ASP.NET         |

# <a name="net-core-1xtabnetcore1x"></a>[.NET Core 1.x](#tab/netcore1x)

此命令包含預設的範本清單。 使用 `dotnet new -all` 以取得可用範本的清單。 下表顯示隨 .NET Core SDK 1.0.1 預先安裝的範本。 範本的預設語言會顯示在方括號內。

| 範本            | 簡短名稱    | 語言 | Tags           |
|----------------------|---------------|----------|----------------|
| 主控台應用程式  | `console`     | [C#]、F# | 通用/主控台 |
| 類別庫        | `classlib`    | [C#]、F# | 通用/程式庫 |
| 單元測試專案    | `mstest`      | [C#]、F# | 測試/MSTest    |
| xUnit 測試專案   | `xunit`       | [C#]、F# | 測試/XUnit     |
| 空的 ASP.NET Core   | `web`         | [C#]     | Web/空白      |
| ASP.NET Core Web 應用程式 | `mvc`         | [C#]、F# | Web/MVC        |
| ASP.NET Core Web API | `webapi`      | [C#]     | Web/WebAPI     |
| NuGet 組態         | `nugetconfig` |          | 組態         |
| Web 組態           | `webconfig`   |          | 組態         |
| 方案檔        | `sln`         |          | 方案       |

---

## <a name="options"></a>選項

# <a name="net-core-22tabnetcore22"></a>[.NET Core 2.2](#tab/netcore22)

`--dry-run`

若指定的命令會導致建立範本，則顯示執行時會發生的情況摘要。

`--force`

強制產生內容，即使它會變更現有的檔案。 當輸出目錄中已包含專案時，這是必要選項。

`-h|--help`

印出命令的說明。 可針對 `dotnet new` 命令本身或任何範本 (例如 `dotnet new mvc --help`) 叫用。

`-i|--install <PATH|NUGET_ID>`

安裝 `PATH` 或 `NUGET_ID` 提供的來源或範本套件。 若您想要安裝預先發行版本的範本套件，就必須以 `<package-name>::<package-version>` 的格式指定版本。 根據預設，`dotnet new` 會傳遞版本的 \*，其代表最新的穩定套件版本。 您可於[範例](#examples)一節查看範例。

如需建立自訂範本的資訊，請參閱 [dotnet new的自訂範本](custom-templates.md)。

`-l|--list`

列出包含指定名稱的範本。 如果針對 `dotnet new` 命令叫用，則會列出指定目錄可能可用的範本。 例如，如果目錄中已包含專案，則不會列出所有專案範本。

`-lang|--language {C#|F#|VB}`

要建立的範本語言。 接受的語言會因範本而有所不同 (請參閱[引數](#arguments)一節中的預設值)。 並非所有範本都適用。

> [!NOTE]
> 某些 Shell 會將 `#` 解譯為特殊字元。 在這些情況下，您需要括住語言參數值，例如 `dotnet new console -lang "F#"`。

`-n|--name <OUTPUT_NAME>`

所建立輸出的名稱。 如果未指定名稱，則會使用目前目錄的名稱。

`--nuget-source`

請指定安裝期間所要使用的 NuGet 來源。

`-o|--output <OUTPUT_DIRECTORY>`

放置所產生輸出的位置。 預設為目前的目錄。

`--type`

根據可用的類型篩選範本。 預先定義的值為「專案」、「項目」或「其他」。

`-u|--uninstall <PATH|NUGET_ID>`

解除安裝 `PATH` 或 `NUGET_ID` 提供的來源或範本套件。 當您排除 `<PATH|NUGET_ID>` 值時，會顯示目前已安裝的所有範本組件及其相關聯範本。

> [!NOTE]
> 若要使用 `PATH` 將範本解除安裝，您需要使路徑成為完整路徑。 例如，*C:/Users/\<USER>/Documents/Templates/GarciaSoftware.ConsoleTemplate.CSharp* 將有效，但來自包含資料夾的 *./GarciaSoftware.ConsoleTemplate.CSharp* 將無效。
> 此外，請勿在範本路徑中包含最終結尾目錄斜線。

# <a name="net-core-21tabnetcore21"></a>[.NET Core 2.1](#tab/netcore21)

`--force`

強制產生內容，即使它會變更現有的檔案。 當輸出目錄中已包含專案時，這是必要選項。

`-h|--help`

印出命令的說明。 可針對 `dotnet new` 命令本身或任何範本 (例如 `dotnet new mvc --help`) 叫用。

`-i|--install <PATH|NUGET_ID>`

安裝 `PATH` 或 `NUGET_ID` 提供的來源或範本套件。 若您想要安裝預先發行版本的範本套件，就必須以 `<package-name>::<package-version>` 的格式指定版本。 根據預設，`dotnet new` 會傳遞版本的 \*，其代表最新的穩定套件版本。 您可於[範例](#examples)一節查看範例。

如需建立自訂範本的資訊，請參閱 [dotnet new的自訂範本](custom-templates.md)。

`-l|--list`

列出包含指定名稱的範本。 如果針對 `dotnet new` 命令叫用，則會列出指定目錄可能可用的範本。 例如，如果目錄中已包含專案，則不會列出所有專案範本。

`-lang|--language {C#|F#|VB}`

要建立的範本語言。 接受的語言會因範本而有所不同 (請參閱[引數](#arguments)一節中的預設值)。 並非所有範本都適用。

> [!NOTE]
> 某些 Shell 會將 `#` 解譯為特殊字元。 在這些情況下，您需要括住語言參數值，例如 `dotnet new console -lang "F#"`。

`-n|--name <OUTPUT_NAME>`

所建立輸出的名稱。 如果未指定名稱，則會使用目前目錄的名稱。

`--nuget-source`

請指定安裝期間所要使用的 NuGet 來源。

`-o|--output <OUTPUT_DIRECTORY>`

放置所產生輸出的位置。 預設為目前的目錄。

`--type`

根據可用的類型篩選範本。 預先定義的值為「專案」、「項目」或「其他」。

`-u|--uninstall <PATH|NUGET_ID>`

解除安裝 `PATH` 或 `NUGET_ID` 提供的來源或範本套件。

> [!NOTE]
> 若要使用 `PATH` 將範本解除安裝，您需要使路徑成為完整路徑。 例如，*C:/Users/\<USER>/Documents/Templates/GarciaSoftware.ConsoleTemplate.CSharp* 將有效，但來自包含資料夾的 *./GarciaSoftware.ConsoleTemplate.CSharp* 將無效。
> 此外，請勿在範本路徑中包含最終結尾目錄斜線。

# <a name="net-core-20tabnetcore20"></a>[.NET Core 2.0](#tab/netcore20)

`--force`

強制產生內容，即使它會變更現有的檔案。 當輸出目錄中已包含專案時，這是必要選項。

`-h|--help`

印出命令的說明。 可針對 `dotnet new` 命令本身或任何範本 (例如 `dotnet new mvc --help`) 叫用。

`-i|--install <PATH|NUGET_ID>`

安裝 `PATH` 或 `NUGET_ID` 提供的來源或範本套件。 若您想要安裝預先發行版本的範本套件，就必須以 `<package-name>::<package-version>` 的格式指定版本。 根據預設，`dotnet new` 會傳遞版本的 \*，其代表最新的穩定套件版本。 您可於[範例](#examples)一節查看範例。

如需建立自訂範本的資訊，請參閱 [dotnet new的自訂範本](custom-templates.md)。

`-l|--list`

列出包含指定名稱的範本。 如果針對 `dotnet new` 命令叫用，則會列出指定目錄可能可用的範本。 例如，如果目錄中已包含專案，則不會列出所有專案範本。

`-lang|--language {C#|F#|VB}`

要建立的範本語言。 接受的語言會因範本而有所不同 (請參閱[引數](#arguments)一節中的預設值)。 並非所有範本都適用。

> [!NOTE]
> 某些 Shell 會將 `#` 解譯為特殊字元。 在這些情況下，您需要括住語言參數值，例如 `dotnet new console -lang "F#"`。

`-n|--name <OUTPUT_NAME>`

所建立輸出的名稱。 如果未指定名稱，則會使用目前目錄的名稱。

`-o|--output <OUTPUT_DIRECTORY>`

放置所產生輸出的位置。 預設為目前的目錄。

`--type`

根據可用的類型篩選範本。 預先定義的值為「專案」、「項目」或「其他」。

`-u|--uninstall <PATH|NUGET_ID>`

解除安裝 `PATH` 或 `NUGET_ID` 提供的來源或範本套件。

> [!NOTE]
> 若要使用來源 `PATH` 將範本解除安裝，您需要使路徑成為完整路徑。 例如，*C:/Users/\<USER>/Documents/Templates/GarciaSoftware.ConsoleTemplate.CSharp* 將有效，但來自包含資料夾的 *./GarciaSoftware.ConsoleTemplate.CSharp* 將無效。 此外，請勿在範本路徑中包含最終結尾目錄斜線。
> 
> 如果您無法判斷將範本解除安裝需要 `PATH` 或是 `NUGET_ID` 引數，在沒有引數的情況下執行 `dotnet new --uninstall` 會列出所有已安裝的範本，以及將它們解除安裝所需的引數。

# <a name="net-core-1xtabnetcore1x"></a>[.NET Core 1.x](#tab/netcore1x)

`-all|--show-all`

單獨在 `dotnet new` 命令的內容中執行時，顯示特定專案類型的所有範本。 在特定範本的內容中執行時 (例如 `dotnet new web -all`)，`-all` 會解譯為強制建立旗標。 當輸出目錄中已包含專案時，這是必要選項。

`-h|--help`

印出命令的說明。 可針對 `dotnet new` 命令本身或任何範本 (例如 `dotnet new mvc --help`) 叫用。

`-l|--list`

列出包含指定名稱的範本。 如果針對 `dotnet new` 命令叫用，則會列出指定目錄可能可用的範本。 例如，如果目錄中已包含專案，則不會列出所有專案範本。

`-lang|--language {C#|F#}`

要建立的範本語言。 接受的語言會因範本而有所不同 (請參閱[引數](#arguments)一節中的預設值)。 並非所有範本都適用。

> [!NOTE]
> 某些 Shell 會將 `#` 解譯為特殊字元。 在這些情況下，您需要括住語言參數值，例如 `dotnet new console -lang "F#"`。

`-n|--name <OUTPUT_NAME>`

所建立輸出的名稱。 如果未指定名稱，則會使用目前目錄的名稱。

`-o|--output <OUTPUT_DIRECTORY>`

放置所產生輸出的位置。 預設為目前的目錄。

---

## <a name="template-options"></a>範本選項

每個專案範本都可能會有其他可用的選項。 核心範本有下列額外選項：

# <a name="net-core-22tabnetcore22"></a>[.NET Core 2.2](#tab/netcore22)

**主控台**

`--langVersion <VERSION_NUMBER>` - 在建立的專案檔中設定 `LangVersion` 屬性。 例如，使用 `--langVersion 7.3` 可使用 C# 7.3。 不支援 F#。

`--no-restore` - 專案建立期間不執行隱含還原。

**angular、react、reactredux**

`--exclude-launch-settings` - 從產生的範本中排除 *launchSettings.json*。

`--no-restore` - 專案建立期間不執行隱含還原。

`--no-https` - 專案不需要 HTTPS。 此選項僅適用於未使用 `IndividualAuth` 或 `OrganizationalAuth` 時。

**razorclasslib**

`--no-restore` - 專案建立期間不執行隱含還原。

**classlib**

`-f|--framework <FRAMEWORK>` - 指定要當成目標的[架構](../../standard/frameworks.md)。 值：`netcoreapp2.2` 建立 .NET Core 類別庫或 `netstandard2.0` 建立 .NET Standard 類別庫。 預設值為 `netstandard2.0`。

`--langVersion <VERSION_NUMBER>` - 在建立的專案檔中設定 `LangVersion` 屬性。 例如，使用 `--langVersion 7.3` 可使用 C# 7.3。 不支援 F#。

`--no-restore` - 專案建立期間不執行隱含還原。

**mstest, xunit**

`-p|--enable-pack` - 使用 [dotnet pack](dotnet-pack.md) 封裝專案。

`--no-restore` - 專案建立期間不執行隱含還原。

**nunit**

`-f|--framework <FRAMEWORK>` - 指定要當成目標的[架構](../../standard/frameworks.md)。 預設值為 `netcoreapp2.1`。

`-p|--enable-pack` - 使用 [dotnet pack](dotnet-pack.md) 封裝專案。

`--no-restore` - 專案建立期間不執行隱含還原。

**PAGE**

`-na|--namespace <NAMESPACE_NAME>` - 所產生程式碼的命名空間。 預設值為 `MyApp.Namespace`。

`-np|--no-pagemodel` - 不使用 PageModel 建立頁面。

**viewimports**

`-na|--namespace <NAMESPACE_NAME>` - 所產生程式碼的命名空間。 預設值為 `MyApp.Namespace`。

**web**

`--exclude-launch-settings` - 從產生的範本中排除 *launchSettings.json*。

`--no-restore` - 專案建立期間不執行隱含還原。

`--no-https` - 專案不需要 HTTPS。 此選項僅適用於未使用 `IndividualAuth` 或 `OrganizationalAuth` 時。

**mvc、webapp**

`-au|--auth <AUTHENTICATION_TYPE>` - 要使用的驗證類型。 可能值為：

- `None` - 無驗證 (預設值)。
- `Individual` - 個別驗證。
- `IndividualB2C` - 使用 Azure AD B2C 的個別驗證。
- `SingleOrg` - 單一租用戶的組織驗證。
- `MultiOrg` - 多個租用戶的組織驗證。
- `Windows` - Windows 驗證。

`--aad-b2c-instance <INSTANCE>` - 要連接的 Azure Active Directory B2C 執行個體。 搭配 `IndividualB2C` 驗證使用。 預設值為 `https://login.microsoftonline.com/tfp/`。

`-ssp|--susi-policy-id <ID>` - 此專案的登入及註冊原則識別碼。 搭配 `IndividualB2C` 驗證使用。

`-rp|--reset-password-policy-id <ID>` - 此專案的重設密碼原則識別碼。 搭配 `IndividualB2C` 驗證使用。

`-ep|--edit-profile-policy-id <ID>` - 此專案的編輯設定檔原則識別碼。 搭配 `IndividualB2C` 驗證使用。

`--aad-instance <INSTANCE>` - 要連接的 Azure Active Directory 執行個體。 搭配 `SingleOrg` 或 `MultiOrg` 驗證使用。 預設值為 `https://login.microsoftonline.com/`。

`--client-id <ID>` - 此專案的用戶端識別碼。 搭配 `IndividualB2C`、`SingleOrg` 或 `MultiOrg` 驗證使用。 預設值為 `11111111-1111-1111-11111111111111111`。

`--domain <DOMAIN>` - 目錄租用戶的網域。 搭配 `SingleOrg` 或 `IndividualB2C` 驗證使用。 預設值為 `qualified.domain.name`。

`--tenant-id <ID>` - 要連接的目錄 TenantId 識別碼。 搭配 `SingleOrg` 驗證使用。 預設值為 `22222222-2222-2222-2222-222222222222`。

`--callback-path <PATH>` - 重新導向 URI 的應用程式基底路徑內的要求路徑。 搭配 `SingleOrg` 或 `IndividualB2C` 驗證使用。 預設值為 `/signin-oidc`。

`-r|--org-read-access` - 允許此應用程式對目錄的讀取權限。 僅適用於 `SingleOrg` 或 `MultiOrg` 驗證。

`--exclude-launch-settings` - 從產生的範本中排除 *launchSettings.json*。

`--no-https` - 專案不需要 HTTPS。 `app.UseHsts` 和 `app.UseHttpsRedirection` 並未新增至 `Startup.Configure`。 此選項僅適用於未使用 `Individual`、`IndividualB2C`、`SingleOrg` 或 `MultiOrg` 時。

`-uld|--use-local-db` - 指定應該使用 LocalDB，不使用 SQLite。 僅適用於 `Individual` 或 `IndividualB2C` 驗證。

`--no-restore` - 專案建立期間不執行隱含還原。

**webapi**

`-au|--auth <AUTHENTICATION_TYPE>` - 要使用的驗證類型。 可能值為：

- `None` - 無驗證 (預設值)。
- `IndividualB2C` - 使用 Azure AD B2C 的個別驗證。
- `SingleOrg` - 單一租用戶的組織驗證。
- `Windows` - Windows 驗證。

`--aad-b2c-instance <INSTANCE>` - 要連接的 Azure Active Directory B2C 執行個體。 搭配 `IndividualB2C` 驗證使用。 預設值為 `https://login.microsoftonline.com/tfp/`。

`-ssp|--susi-policy-id <ID>` - 此專案的登入及註冊原則識別碼。 搭配 `IndividualB2C` 驗證使用。

`--aad-instance <INSTANCE>` - 要連接的 Azure Active Directory 執行個體。 搭配 `SingleOrg` 驗證使用。 預設值為 `https://login.microsoftonline.com/`。

`--client-id <ID>` - 此專案的用戶端識別碼。 搭配 `IndividualB2C` 或 `SingleOrg` 驗證使用。 預設值為 `11111111-1111-1111-11111111111111111`。

`--domain <DOMAIN>` - 目錄租用戶的網域。 搭配 `SingleOrg` 或 `IndividualB2C` 驗證使用。 預設值為 `qualified.domain.name`。

`--tenant-id <ID>` - 要連接的目錄 TenantId 識別碼。 搭配 `SingleOrg` 驗證使用。 預設值為 `22222222-2222-2222-2222-222222222222`。

`-r|--org-read-access` - 允許此應用程式對目錄的讀取權限。 僅適用於 `SingleOrg` 或 `MultiOrg` 驗證。

`--exclude-launch-settings` - 從產生的範本中排除 *launchSettings.json*。

`--no-https` - 專案不需要 HTTPS。 `app.UseHsts` 和 `app.UseHttpsRedirection` 並未新增至 `Startup.Configure`。 此選項僅適用於未使用 `Individual`、`IndividualB2C`、`SingleOrg` 或 `MultiOrg` 時。

`-uld|--use-local-db` - 指定應該使用 LocalDB，不使用 SQLite。 僅適用於 `Individual` 或 `IndividualB2C` 驗證。

`--no-restore` - 專案建立期間不執行隱含還原。

**globaljson**

`--sdk-version <VERSION_NUMBER>` - 指定要在 *global.json* 檔案中使用的 .NET Core SDK 版本。

# <a name="net-core-21tabnetcore21"></a>[.NET Core 2.1](#tab/netcore21)

**主控台, angular, react, reactredux, razorclasslib**

`--no-restore` - 專案建立期間不執行隱含還原。

**classlib**

`-f|--framework <FRAMEWORK>` - 指定要當成目標的[架構](../../standard/frameworks.md)。 值：`netcoreapp2.1` 建立 .NET Core 類別庫或 `netstandard2.0` 建立 .NET Standard 類別庫。 預設值為 `netstandard2.0`。

`--no-restore` - 專案建立期間不執行隱含還原。

**mstest, xunit**

`-p|--enable-pack` - 使用 [dotnet pack](dotnet-pack.md) 封裝專案。

`--no-restore` - 專案建立期間不執行隱含還原。

**globaljson**

`--sdk-version <VERSION_NUMBER>` - 指定要在 *global.json* 檔案中使用的 .NET Core SDK 版本。

**web**

`--exclude-launch-settings` - 從產生的範本中排除 *launchSettings.json*。

`--no-restore` - 專案建立期間不執行隱含還原。

`--no-https` - 專案不需要 HTTPS。 此選項僅適用於未使用 `IndividualAuth` 或 `OrganizationalAuth` 時。

**webapi**

`-au|--auth <AUTHENTICATION_TYPE>` - 要使用的驗證類型。 可能值為：

- `None` - 無驗證 (預設值)。
- `IndividualB2C` - 使用 Azure AD B2C 的個別驗證。
- `SingleOrg` - 單一租用戶的組織驗證。
- `Windows` - Windows 驗證。

`--aad-b2c-instance <INSTANCE>` - 要連接的 Azure Active Directory B2C 執行個體。 搭配 `IndividualB2C` 驗證使用。 預設值為 `https://login.microsoftonline.com/tfp/`。

`-ssp|--susi-policy-id <ID>` - 此專案的登入及註冊原則識別碼。 搭配 `IndividualB2C` 驗證使用。

`--aad-instance <INSTANCE>` - 要連接的 Azure Active Directory 執行個體。 搭配 `SingleOrg` 驗證使用。 預設值為 `https://login.microsoftonline.com/`。

`--client-id <ID>` - 此專案的用戶端識別碼。 搭配 `IndividualB2C` 或 `SingleOrg` 驗證使用。 預設值為 `11111111-1111-1111-11111111111111111`。

`--domain <DOMAIN>` - 目錄租用戶的網域。 搭配 `SingleOrg` 或 `IndividualB2C` 驗證使用。 預設值為 `qualified.domain.name`。

`--tenant-id <ID>` - 要連接的目錄 TenantId 識別碼。 搭配 `SingleOrg` 驗證使用。 預設值為 `22222222-2222-2222-2222-222222222222`。

`-r|--org-read-access` - 允許此應用程式對目錄的讀取權限。 僅適用於 `SingleOrg` 或 `MultiOrg` 驗證。

`--exclude-launch-settings` - 從產生的範本中排除 *launchSettings.json*。

`-uld|--use-local-db` - 指定應該使用 LocalDB，不使用 SQLite。 僅適用於 `Individual` 或 `IndividualB2C` 驗證。

`--no-restore` - 專案建立期間不執行隱含還原。

`--no-https` - 專案不需要 HTTPS。 `app.UseHsts` 和 `app.UseHttpsRedirection` 並未新增至 `Startup.Configure`。 此選項僅適用於未使用 `Individual`、`IndividualB2C`、`SingleOrg` 或 `MultiOrg` 時。

**mvc, razor**

`-au|--auth <AUTHENTICATION_TYPE>` - 要使用的驗證類型。 可能值為：

- `None` - 無驗證 (預設值)。
- `Individual` - 個別驗證。
- `IndividualB2C` - 使用 Azure AD B2C 的個別驗證。
- `SingleOrg` - 單一租用戶的組織驗證。
- `MultiOrg` - 多個租用戶的組織驗證。
- `Windows` - Windows 驗證。

`--aad-b2c-instance <INSTANCE>` - 要連接的 Azure Active Directory B2C 執行個體。 搭配 `IndividualB2C` 驗證使用。 預設值為 `https://login.microsoftonline.com/tfp/`。

`-ssp|--susi-policy-id <ID>` - 此專案的登入及註冊原則識別碼。 搭配 `IndividualB2C` 驗證使用。

`-rp|--reset-password-policy-id <ID>` - 此專案的重設密碼原則識別碼。 搭配 `IndividualB2C` 驗證使用。

`-ep|--edit-profile-policy-id <ID>` - 此專案的編輯設定檔原則識別碼。 搭配 `IndividualB2C` 驗證使用。

`--aad-instance <INSTANCE>` - 要連接的 Azure Active Directory 執行個體。 搭配 `SingleOrg` 或 `MultiOrg` 驗證使用。 預設值為 `https://login.microsoftonline.com/`。

`--client-id <ID>` - 此專案的用戶端識別碼。 搭配 `IndividualB2C`、`SingleOrg` 或 `MultiOrg` 驗證使用。 預設值為 `11111111-1111-1111-11111111111111111`。

`--domain <DOMAIN>` - 目錄租用戶的網域。 搭配 `SingleOrg` 或 `IndividualB2C` 驗證使用。 預設值為 `qualified.domain.name`。

`--tenant-id <ID>` - 要連接的目錄 TenantId 識別碼。 搭配 `SingleOrg` 驗證使用。 預設值為 `22222222-2222-2222-2222-222222222222`。

`--callback-path <PATH>` - 重新導向 URI 的應用程式基底路徑內的要求路徑。 搭配 `SingleOrg` 或 `IndividualB2C` 驗證使用。 預設值為 `/signin-oidc`。

`-r|--org-read-access` - 允許此應用程式對目錄的讀取權限。 僅適用於 `SingleOrg` 或 `MultiOrg` 驗證。

`--exclude-launch-settings` - 從產生的範本中排除 *launchSettings.json*。

`--use-browserlink` - 專案中包含 BrowserLink。

`-uld|--use-local-db` - 指定應該使用 LocalDB，不使用 SQLite。 僅適用於 `Individual` 或 `IndividualB2C` 驗證。

`--no-restore` - 專案建立期間不執行隱含還原。

`--no-https` - 專案不需要 HTTPS。 `app.UseHsts` 和 `app.UseHttpsRedirection` 並未新增至 `Startup.Configure`。 此選項僅適用於未使用 `Individual`、`IndividualB2C`、`SingleOrg` 或 `MultiOrg` 時。

**PAGE**

`-na|--namespace <NAMESPACE_NAME>` - 所產生程式碼的命名空間。 預設值為 `MyApp.Namespace`。

`-np|--no-pagemodel` - 不使用 PageModel 建立頁面。

**viewimports**

`-na|--namespace <NAMESPACE_NAME>` - 所產生程式碼的命名空間。 預設值為 `MyApp.Namespace`。

# <a name="net-core-20tabnetcore20"></a>[.NET Core 2.0](#tab/netcore20)

**主控台, angular, react, reactredux**

`--no-restore` - 專案建立期間不執行隱含還原。

**classlib**

`-f|--framework <FRAMEWORK>` - 指定要當成目標的[架構](../../standard/frameworks.md)。 值：`netcoreapp2.0` 建立 .NET Core 類別庫或 `netstandard2.0` 建立 .NET Standard 類別庫。 預設值為 `netstandard2.0`。

`--no-restore` - 專案建立期間不執行隱含還原。

**mstest, xunit**

`-p|--enable-pack` - 使用 [dotnet pack](dotnet-pack.md) 封裝專案。

`--no-restore` - 專案建立期間不執行隱含還原。

**globaljson**

`--sdk-version <VERSION_NUMBER>` - 指定要在 *global.json* 檔案中使用的 .NET Core SDK 版本。

**web**

`--use-launch-settings` - 在產生的範本輸出中包含 *launchSettings.json*。

`--no-restore` - 專案建立期間不執行隱含還原。

**webapi**

`-au|--auth <AUTHENTICATION_TYPE>` - 要使用的驗證類型。 可能值為：

- `None` - 無驗證 (預設值)。
- `IndividualB2C` - 使用 Azure AD B2C 的個別驗證。
- `SingleOrg` - 單一租用戶的組織驗證。
- `Windows` - Windows 驗證。

`--aad-b2c-instance <INSTANCE>` - 要連接的 Azure Active Directory B2C 執行個體。 搭配 `IndividualB2C` 驗證使用。 預設值為 `https://login.microsoftonline.com/tfp/`。

`-ssp|--susi-policy-id <ID>` - 此專案的登入及註冊原則識別碼。 搭配 `IndividualB2C` 驗證使用。

`--aad-instance <INSTANCE>` - 要連接的 Azure Active Directory 執行個體。 搭配 `SingleOrg` 驗證使用。 預設值為 `https://login.microsoftonline.com/`。

`--client-id <ID>` - 此專案的用戶端識別碼。 搭配 `IndividualB2C` 或 `SingleOrg` 驗證使用。 預設值為 `11111111-1111-1111-11111111111111111`。

`--domain <DOMAIN>` - 目錄租用戶的網域。 搭配 `SingleOrg` 或 `IndividualB2C` 驗證使用。 預設值為 `qualified.domain.name`。

`--tenant-id <ID>` - 要連接的目錄 TenantId 識別碼。 搭配 `SingleOrg` 驗證使用。 預設值為 `22222222-2222-2222-2222-222222222222`。

`-r|--org-read-access` - 允許此應用程式對目錄的讀取權限。 僅適用於 `SingleOrg` 或 `MultiOrg` 驗證。

`--use-launch-settings` - 在產生的範本輸出中包含 *launchSettings.json*。

`-uld|--use-local-db` - 指定應該使用 LocalDB，不使用 SQLite。 僅適用於 `Individual` 或 `IndividualB2C` 驗證。

`--no-restore` - 專案建立期間不執行隱含還原。

**mvc, razor**

`-au|--auth <AUTHENTICATION_TYPE>` - 要使用的驗證類型。 可能值為：

- `None` - 無驗證 (預設值)。
- `Individual` - 個別驗證。
- `IndividualB2C` - 使用 Azure AD B2C 的個別驗證。
- `SingleOrg` - 單一租用戶的組織驗證。
- `MultiOrg` - 多個租用戶的組織驗證。
- `Windows` - Windows 驗證。

`--aad-b2c-instance <INSTANCE>` - 要連接的 Azure Active Directory B2C 執行個體。 搭配 `IndividualB2C` 驗證使用。 預設值為 `https://login.microsoftonline.com/tfp/`。

`-ssp|--susi-policy-id <ID>` - 此專案的登入及註冊原則識別碼。 搭配 `IndividualB2C` 驗證使用。

`-rp|--reset-password-policy-id <ID>` - 此專案的重設密碼原則識別碼。 搭配 `IndividualB2C` 驗證使用。

`-ep|--edit-profile-policy-id <ID>` - 此專案的編輯設定檔原則識別碼。 搭配 `IndividualB2C` 驗證使用。

`--aad-instance <INSTANCE>` - 要連接的 Azure Active Directory 執行個體。 搭配 `SingleOrg` 或 `MultiOrg` 驗證使用。 預設值為 `https://login.microsoftonline.com/`。

`--client-id <ID>` - 此專案的用戶端識別碼。 搭配 `IndividualB2C`、`SingleOrg` 或 `MultiOrg` 驗證使用。 預設值為 `11111111-1111-1111-11111111111111111`。

`--domain <DOMAIN>` - 目錄租用戶的網域。 搭配 `SingleOrg` 或 `IndividualB2C` 驗證使用。 預設值為 `qualified.domain.name`。

`--tenant-id <ID>` - 要連接的目錄 TenantId 識別碼。 搭配 `SingleOrg` 驗證使用。 預設值為 `22222222-2222-2222-2222-222222222222`。

`--callback-path <PATH>` - 重新導向 URI 的應用程式基底路徑內的要求路徑。 搭配 `SingleOrg` 或 `IndividualB2C` 驗證使用。 預設值為 `/signin-oidc`。

`-r|--org-read-access` - 允許此應用程式對目錄的讀取權限。 僅適用於 `SingleOrg` 或 `MultiOrg` 驗證。

`--use-launch-settings` - 在產生的範本輸出中包含 *launchSettings.json*。

`--use-browserlink` - 專案中包含 BrowserLink。

`-uld|--use-local-db` - 指定應該使用 LocalDB，不使用 SQLite。 僅適用於 `Individual` 或 `IndividualB2C` 驗證。

`--no-restore` - 專案建立期間不執行隱含還原。

**PAGE**

`-na|--namespace <NAMESPACE_NAME>` - 所產生程式碼的命名空間。 預設值為 `MyApp.Namespace`。

`-np|--no-pagemodel` - 不使用 PageModel 建立頁面。

**viewimports**

`-na|--namespace <NAMESPACE_NAME>` - 所產生程式碼的命名空間。 預設值為 `MyApp.Namespace`。

# <a name="net-core-1xtabnetcore1x"></a>[.NET Core 1.x](#tab/netcore1x)

**console、xunit、mstest、web、webapi**

`-f|--framework <FRAMEWORK>` - 指定要當成目標的[架構](../../standard/frameworks.md)。 值：`netcoreapp1.0` 或 `netcoreapp1.1`。 預設值為 `netcoreapp1.0`。

**classlib**

`-f|--framework <FRAMEWORK>` - 指定要當成目標的[架構](../../standard/frameworks.md)。 值：`netcoreapp1.0`、`netcoreapp1.1`，或 `netstandard1.0` 到 `netstandard1.6`。 預設值為 `netstandard1.4`。

**mvc**

`-f|--framework <FRAMEWORK>` - 指定要當成目標的[架構](../../standard/frameworks.md)。 值：`netcoreapp1.0` 或 `netcoreapp1.1`。 預設值為 `netcoreapp1.0`。

`-au|--auth <AUTHENTICATION_TYPE>` - 要使用的驗證類型。 值：`None` 或 `Individual`。 預設值為 `None`。

`-uld|--use-local-db` - 指定是否要使用 LocalDB，而不是 SQLite。 值：`true` 或 `false`。 預設值為 `false`。

---

## <a name="examples"></a>範例

藉由指定範本名稱，建立 C# 主控台應用程式專案：

`dotnet new "Console Application"`

在目前的目錄中建立 F# 主控台應用程式專案：

`dotnet new console -lang F#`

在指定的目錄中建立 .NET Standard 類別庫專案 (僅 .NET Core SDK 2.0 或更新版本提供)：

`dotnet new classlib -lang VB -o MyLibrary`

未經驗證即在目前目錄中建立新的 ASP.NET Core C# MVC 專案：

`dotnet new mvc -au None`

建立新的 xUnit 專案：

`dotnet new xunit`

列出 MVC 可用的所有範本：

`dotnet new mvc -l`

列出所有符合 *we* 子字串的範本。 找不到完全相符的項目，因此會對 [簡短名稱] 和 [名稱] 兩欄執行子字串比對作業。

`dotnet new we -l`

嘗試叫用符合 *ng* 的範本。 如果無法判別單一相符項目，則列出部分相符的範本。

`dotnet new ng`

安裝適用於 ASP.NET Core 的單頁應用程式範本 2.0 版 (僅適用於 .NET Core SDK 1.1 及更新版本的命令選項)：

`dotnet new -i Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0`

在目前目錄中建立 *global.json*，並將 SDK 版本設為 2.0.0 (只能用於.NET Core SDK 2.0 或更新版本)：

`dotnet new globaljson --sdk-version 2.0.0`

## <a name="see-also"></a>另請參閱

- [dotnet new 的自訂範本](custom-templates.md)
- [建立適用於 dotnet new 的自訂範本](../tutorials/create-custom-template.md)
- [dotnet/dotnet-template-samples GitHub repo](https://github.com/dotnet/dotnet-template-samples) (dotnet/dotnet-template-samples GitHub 存放庫)
- [dotnet new 的可用範本](https://github.com/dotnet/templating/wiki/Available-templates-for-dotnet-new)
