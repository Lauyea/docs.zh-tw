---
title: Windows Form 組態區段
ms.date: 04/07/2017
ms.assetid: 6eb142d5-fc98-40e2-9d90-84733f2a27ba
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: e76e11ef8bb39d72cb16655c948354bc326e75bc
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2019
ms.locfileid: "69913091"
---
# <a name="windows-forms-configuration-section"></a>Windows Form 組態區段
Windows Forms 組態設定允許 Windows Forms 應用程式儲存並擷取有關自訂應用程式設定 (例如多重監視器支援、高 DPI 支援，以及其他預先定義的組態設定) 的資訊。

Windows Forms 應用程式組態設定是儲存在應用程式組態檔的 `System.Windows.Forms.ApplicationConfigurationSection` 元素中。

## <a name="syntax"></a>語法

```xml
<configuration>
  <System.Windows.Forms.ApplicationConfigurationSection>
  ...
  </System.Windows.Forms.ApplicationConfigurationSection>
</configuration>
```

## <a name="attributes-and-elements"></a>屬性和元素

下列章節說明屬性、子元素和父元素。

### <a name="attributes"></a>屬性

無。

### <a name="child-elements"></a>子元素

項目  |說明 |
---------|---------|
[`<add>`](windows-forms-add-configuration-element.md) | 新增具有指定值的組態設定金鑰 |

### <a name="parent-elements"></a>父元素

元素  |描述 |
---------|---------|
[\<configuration>](../configuration-element.md) | 通用語言執行平台和 Windows Forms 應用程式所使用之每個組態檔中的根元素 |

## <a name="remarks"></a>備註

從 .NET Framework 4.7 開始，`<System.Windows.Forms.ApplicationConfigurationSection>` 元素能允許您設定 Windows Forms 應用程式，以利用 .NET Framework 最新版本中的新增功能。 

`<System.Windows.Forms.ApplicationConfigurationSection>` 元素可以包含一或多個子 [`<add>`](windows-forms-add-configuration-element.md) 元素，每個子元素都能定義一個特定的組態設定。

## <a name="see-also"></a>另請參閱

- [組態檔結構描述](../index.md)
- [Windows Forms 中的高 DPI 支援](../../../winforms/high-dpi-support-in-windows-forms.md)
