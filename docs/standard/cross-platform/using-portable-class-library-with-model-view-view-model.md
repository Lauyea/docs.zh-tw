---
title: 搭配 Model-View-View 模型使用可攜式類別庫
ms.date: 07/18/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Portable Class Library [.NET Framework], and MVVM
- MVVM, and Portable Class Library
ms.assetid: 41a0b9f8-15a2-431a-bc35-e310b2953b03
author: mairaw
ms.author: mairaw
ms.openlocfilehash: b53be90764c6537fb27cb1b5ed781a68e69effa0
ms.sourcegitcommit: b1cfd260928d464d91e20121f9bdba7611c94d71
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2019
ms.locfileid: "67504670"
---
# <a name="using-portable-class-library-with-model-view-view-model"></a>搭配 Model-View-View 模型使用可攜式類別庫
您可以使用.NET Framework[可攜式類別庫](../../../docs/standard/cross-platform/cross-platform-development-with-the-portable-class-library.md)實作模型檢視 View Model (MVVM) 模式，並跨多個平台共用的組件。

[!INCLUDE[standard](../../../includes/pcl-to-standard.md)]

 MVVM 是將使用者介面與基礎商務邏輯隔離的應用程式模式。 您可以在 Visual Studio 2012 中，可攜式類別庫專案中實作模型和檢視模型類別，然後再建立自訂的不同平台的檢視。 這個方法讓您只需撰寫資料模型和商務邏輯一次，就可以從 .NET Framework、Silverlight、Windows Phone 和 [!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)]應用程式使用該程式碼，如下圖所示。

 ![顯示跨平台的可攜式類別庫搭配 MVVM 共用組件。](./media/using-portable-class-library-with-model-view-view-model/mvvm-share-assemblies-across-platforms.png)

 本主題不提供 MVVM 模式的一般資訊。 此外，它只會提供有關如何實作 MVVM 使用可攜式類別庫的資訊。 如需 MVVM 的詳細資訊，請參閱[MVVM 快速入門使用 Prism Library 5.0 for WPF](https://docs.microsoft.com/previous-versions/msp-n-p/gg430857(v=pandp.40))。

## <a name="classes-that-support-mvvm"></a>支援 MVVM 類別
 .NET Framework 4.5 為目標時[!INCLUDE[net_win8_profile](../../../includes/net-win8-profile-md.md)]，Silverlight 或 Windows Phone 7.5，可攜式類別庫專案，下列類別可供實作 MVVM 模式：

- <xref:System.Collections.ObjectModel.ObservableCollection%601?displayProperty=nameWithType> 類別

- <xref:System.Collections.ObjectModel.ReadOnlyObservableCollection%601?displayProperty=nameWithType> 類別

- <xref:System.Collections.Specialized.INotifyCollectionChanged?displayProperty=nameWithType> 類別

- <xref:System.Collections.Specialized.NotifyCollectionChangedAction?displayProperty=nameWithType> 類別

- <xref:System.Collections.Specialized.NotifyCollectionChangedEventArgs?displayProperty=nameWithType> 類別

- <xref:System.Collections.Specialized.NotifyCollectionChangedEventHandler?displayProperty=nameWithType> 類別

- <xref:System.ComponentModel.DataErrorsChangedEventArgs?displayProperty=nameWithType> 類別

- <xref:System.ComponentModel.INotifyDataErrorInfo?displayProperty=nameWithType> 類別

- <xref:System.ComponentModel.INotifyPropertyChanged?displayProperty=nameWithType> 類別

- <xref:System.Windows.Input.ICommand?displayProperty=nameWithType> 類別

- 中的所有類別<xref:System.ComponentModel.DataAnnotations?displayProperty=nameWithType>命名空間

## <a name="implementing-mvvm"></a>實作 MVVM
 若要實作 MVVM，通常會建立模型和檢視模型在可攜式類別庫專案中，因為非可攜式專案無法參考可攜式類別庫專案。 模型和檢視模型可以是相同專案中，或在不同的專案中。 如果您使用不同的專案，請從檢視模型專案中將參考新增到模型專案。

 編譯模型後，當您檢視模型專案時，您會參考包含此檢視的應用程式中的這些組件。 如果檢視互動只具有檢視模型，您只需要參考該組件包含檢視模型。

### <a name="model"></a>型號
 下列範例顯示可能位於可攜式類別庫專案的簡化的模型類別。

 [!code-csharp[PortableClassLibraryMVVM#1](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/customer.cs#1)]
 [!code-vb[PortableClassLibraryMVVM#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customer.vb#1)]

 下列範例顯示簡單的方式來填入、 擷取及更新可攜式類別庫專案中的資料。 在實際的應用程式中，您會從來源，例如 Windows Communication Foundation (WCF) 服務擷取資料。

 [!code-csharp[PortableClassLibraryMVVM#2](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/customerrepository.cs#2)]
 [!code-vb[PortableClassLibraryMVVM#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customerrepository.vb#2)]

### <a name="view-model"></a>檢視模型
 實作 MVVM 模式時，經常會加入檢視模型的基底類別。 下列範例顯示基底類別。

 [!code-csharp[PortableClassLibraryMVVM#3](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/viewmodelbase.cs#3)]
 [!code-vb[PortableClassLibraryMVVM#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/viewmodelbase.vb#3)]

 實作<xref:System.Windows.Input.ICommand>介面經常搭配 MVVM 模式。 下列範例會示範 <xref:System.Windows.Input.ICommand> 介面的實作。

 [!code-csharp[PortableClassLibraryMVVM#4](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/relaycommand.cs#4)]
 [!code-vb[PortableClassLibraryMVVM#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/relaycommand.vb#4)]

 下列範例示範簡化的檢視模型。

 [!code-csharp[PortableClassLibraryMVVM#5](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/mainpageviewmodel.cs#5)]
 [!code-vb[PortableClassLibraryMVVM#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customerviewmodel.vb#5)]  
  
### <a name="view"></a>檢視  
 從.NET Framework 4.5 的應用程式，[!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)]應用程式、 Silverlight 架構應用程式或 Windows Phone 7.5 應用程式，您可以參考包含模型和檢視的模型專案的組件。  然後，您會建立互動的檢視以檢視模型。 下列範例示範簡化的 Windows Presentation Foundation (WPF) 應用程式，以擷取並更新檢視模型中的資料。 您可以建立類似的檢視，silverlight 中的 Windows Phone 或[!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)]應用程式。  
  
 [!code-xaml[PortableClassLibraryMVVM#6](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/mainwindow.xaml#6)]  
  
## <a name="see-also"></a>另請參閱

- [可攜式類別庫](../../../docs/standard/cross-platform/cross-platform-development-with-the-portable-class-library.md)
