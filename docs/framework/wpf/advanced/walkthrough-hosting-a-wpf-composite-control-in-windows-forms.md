---
title: 逐步解說：在 Windows Forms 中裝載 WPF 複合控制項
ms.date: 03/30/2017
helpviewer_keywords:
- hosting WPF content in Windows Forms [WPF]
ms.assetid: 0ac41286-4c1b-4b17-9196-d985cb844ce1
ms.openlocfilehash: a062095885e6c1fc8816a78847968b1c250eabf8
ms.sourcegitcommit: 005980b14629dfc193ff6cdc040800bc75e0a5a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2019
ms.locfileid: "70991454"
---
# <a name="walkthrough-hosting-a-wpf-composite-control-in-windows-forms"></a>逐步解說：在 Windows Forms 中裝載 WPF 複合控制項
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 提供用來建立應用程式的豐富環境。 不過, 當您對程式[!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)]代碼進行大量的投資時, 使用來擴充現有[!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)]的[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]應用程式會更有效率, 而不是從頭重寫。 常見的案例是當您想要在 Windows Forms 應用程式中內嵌一[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]或多個在中執行的控制項。 如需自訂 WPF 控制項的詳細資訊, 請參閱[控制項自訂](../controls/control-customization.md)。  
  
 本逐步解說會引導您完成裝載[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]複合控制項的應用程式, 以在 Windows Forms 應用程式中執行資料輸入。 複合控制項會封裝在 DLL 中。 這個一般程序可以延伸到更複雜的應用程式和控制項。 本逐步解說的設計, 與[逐步解說的外觀和功能大致相同:在 WPF](walkthrough-hosting-a-windows-forms-composite-control-in-wpf.md)中裝載 Windows Forms 複合控制項。 主要差異在於裝載案例相反。  
  
 本逐步解說分為兩節。 第一節簡要說明[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]複合控制項的執行。 第二節詳細討論如何在 Windows Forms 應用程式中裝載複合控制項、接收來自控制項的事件, 以及存取控制項的某些屬性。  
  
 這個逐步解說中所述的工作包括：  
  
- 實作 WPF 複合控制項。  
  
- 實作 Windows Form 主應用程式。  
  
 如需本逐步解說中所述工作的完整程式代碼清單, 請參閱[在 Windows Forms 範例中裝載 WPF 複合控制項](https://github.com/microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/WindowsFormsHostingWpfControl)。  
  
## <a name="prerequisites"></a>必要條件  

若要完成這個逐步解說，您必須具有 Visual Studio。  
  
## <a name="implementing-the-wpf-composite-control"></a>實作 WPF 複合控制項  
 這個[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]範例中使用的複合控制項是簡單的資料輸入表單, 它會採用使用者的名稱和位址。 使用者按一下兩個按鈕中的其中一個來表示工作已完成時，控制項會引發自訂事件，以將該資訊傳回給主應用程式。 下圖顯示轉譯的控制項。 

 下圖顯示 WPF 複合控制項: 

 ![顯示簡單 WPF 控制項的螢幕擷取畫面。](./media/walkthrough-hosting-a-wpf-composite-control-in-windows-forms/windows-presentation-foundation-composite-control.png)  
  
### <a name="creating-the-project"></a>建立專案  
 啟動專案：  
  
1. 啟動[!INCLUDE[TLA#tla_visualstu](../../../../includes/tlasharptla-visualstu-md.md)], 然後開啟 [**新增專案**] 對話方塊。  
  
2. 在 [ C#視覺效果] 和 [Windows 分類] 中, 選取 [ **WPF 使用者控制項程式庫**] 範本。  
  
3. 將新專案命名為 `MyControls`。  
  
4. 在 [位置] 中, 指定方便命名的最上層資料夾, 例如`WindowsFormsHostingWpfControl`。 稍後，您會將主應用程式放在此資料夾中。  
  
5. 按一下 \[確定\] 來建立專案。 預設專案包含名為`UserControl1`的單一控制項。  
  
6. 在方案總管中, `UserControl1`將`MyControl1`重新命名為。  
  
 您的專案應該有下列系統 DLL 的參考。 如果預設未包括所有這些 DLL，則請將它們新增至專案。  
  
- PresentationCore  
  
- PresentationFramework  
  
- 系統  
  
- WindowsBase  
  
### <a name="creating-the-user-interface"></a>建立使用者介面  
 複合[!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)]控制項的會[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]實作為。 複合控制項[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]是由五個<xref:System.Windows.Controls.TextBox>元素所組成。 每<xref:System.Windows.Controls.TextBox>個元素都有<xref:System.Windows.Controls.TextBlock>一個相關聯的元素, 可做為標籤。 底部有兩<xref:System.Windows.Controls.Button>個元素, **[確定] 和 [** **取消**]。 使用者按一下任一個按鈕時，控制項會引發自訂事件，以將資訊傳回給主應用程式。  
  
#### <a name="basic-layout"></a>基本版面配置  
 各種[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]元素會包含<xref:System.Windows.Controls.Grid>在專案中。 您可以使用<xref:System.Windows.Controls.Grid>來排列複合控制項的內容, 就像在 HTML 中`Table`使用專案一樣。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]也有<xref:System.Windows.Documents.Table>元素, 但<xref:System.Windows.Controls.Grid>較輕量且較適合簡單的版面配置工作。  
  
 下列 XAML 顯示基本版面配置。 此 XAML 會藉由指定<xref:System.Windows.Controls.Grid>元素中的資料行和資料列數目, 來定義控制項的整體結構。  
  
 在 MyControl1.xaml 中，將現有的 XAML 取代為下列 XAML。  
  
 [!code-xaml[WindowsFormsHostingWpfControl#101](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml#101)]  
[!code-xaml[WindowsFormsHostingWpfControl#102](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml#102)]  
  
#### <a name="adding-textblock-and-textbox-elements-to-the-grid"></a>將 TextBlock 和 TextBox 項目新增至格線  
 您可以藉[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]由將專案的<xref:System.Windows.Controls.Grid.RowProperty>和<xref:System.Windows.Controls.Grid.ColumnProperty>屬性設定為適當的資料列和資料行編號, 將元素放在方格中。 請記住，資料列和資料行編號是以零起始。 您可以藉由設定其<xref:System.Windows.Controls.Grid.ColumnSpanProperty>屬性, 讓元素跨越多個資料行。 如需<xref:System.Windows.Controls.Grid>元素的詳細資訊, 請參閱[建立 Grid 元素](../controls/how-to-create-a-grid-element.md)。  
  
 下列 XAML 會<xref:System.Windows.Controls.TextBox>以其<xref:System.Windows.Controls.Grid.RowProperty>和<xref:System.Windows.Controls.TextBlock> <xref:System.Windows.Controls.Grid.ColumnProperty>屬性顯示覆合控制項的和元素, 這些專案會設定為在方格中正確地放置元素。  
  
 在 mycontrol1.xaml 中, 于專案內<xref:System.Windows.Controls.Grid>新增下列 xaml。  
  
 [!code-xaml[WindowsFormsHostingWpfControl#103](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml#103)]  
  
#### <a name="styling-the-ui-elements"></a>設定 UI 項目的樣式  
 資料輸入表單上的許多項目都會有類似的外觀，表示它們具有數個屬性的相同設定。 先前的 XAML 會使用<xref:System.Windows.Style>專案來定義專案類別的標準屬性設定, 而不是分別設定每個元素的屬性。 此方法會減少控制項的複雜度，並可讓您透過單一樣式屬性來變更多個項目的外觀。  
  
 元素會包含<xref:System.Windows.Controls.Grid>在專案的<xref:System.Windows.FrameworkElement.Resources%2A>屬性中, 因此控制項中的所有元素都可以使用這些專案。 <xref:System.Windows.Style> 如果樣式名為, 您可以藉由加入<xref:System.Windows.Style>設定為樣式名稱的元素, 將它套用至元素。 未命名的樣式會成為項目的預設樣式。 如需[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]樣式的詳細資訊, 請參閱設定樣式[和範本](../controls/styling-and-templating.md)。  
  
 下列 XAML 會顯示覆合<xref:System.Windows.Style>控制項的元素。 若要查看如何將樣式套用至項目，請參閱先前的 XAML。 例如, 最後一個<xref:System.Windows.Controls.TextBlock>元素`inlineText`具有樣式, 而最後一個<xref:System.Windows.Controls.TextBox>專案使用預設樣式。  
  
 在 mycontrol1.xaml 中, 新增下列 xaml, 緊接在<xref:System.Windows.Controls.Grid> start 元素後面。  
  
 [!code-xaml[WindowsFormsHostingWpfControl#104](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml#104)]  
  
#### <a name="adding-the-ok-and-cancel-buttons"></a>新增 OK 和 Cancel 按鈕  
 複合控制項上的最後一個元素是**OK**和**Cancel** <xref:System.Windows.Controls.Button>專案, 其佔用最後一個資料列的前兩個數據行。 <xref:System.Windows.Controls.Grid> 這些元素會使用常見的事件處理`ButtonClicked`程式, 以及上<xref:System.Windows.Controls.Button>一個 XAML 中定義的預設樣式。  
  
 在 mycontrol1.xaml 中, 于最後一個<xref:System.Windows.Controls.TextBox>元素之後加入下列 xaml。 複合[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]控制項的部分現在已完成。  
  
 [!code-xaml[WindowsFormsHostingWpfControl#105](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml#105)]  
  
### <a name="implementing-the-code-behind-file"></a>實作程式碼後置檔案  
 程式碼後置檔案 MyControl1.xaml.cs 會執行三個基本工作:
  
1. 處理使用者按一下其中一個按鈕時所發生的事件。  
  
2. 從<xref:System.Windows.Controls.TextBox>專案抓取資料, 並將其封裝在自訂事件引數物件中。  
  
3. 引發自訂`OnButtonClick`事件, 它會通知主機使用者已完成, 並將資料傳回給主機。  
  
 此控制項也會公開一些可讓您變更外觀的色彩和字型屬性。 不同于<xref:System.Windows.Forms.Integration.WindowsFormsHost>用來裝載 Windows Forms 控制項<xref:System.Windows.Forms.Integration.ElementHost>的類別, 類別只會公開控制項的<xref:System.Windows.Controls.Panel.Background%2A>屬性。 若要維護此程式碼範例與逐步解說中[所討論之範例之間的相似性:在 WPF](walkthrough-hosting-a-windows-forms-composite-control-in-wpf.md)中裝載 Windows Forms 複合控制項, 控制項會直接公開其餘的屬性。  
  
#### <a name="the-basic-structure-of-the-code-behind-file"></a>程式碼後置檔案的基本結構  
 程式碼後置檔案是由單一命名空間`MyControls`所組成, 其中包含兩個類別: `MyControlEventArgs` `MyControl1`和。  
  
```csharp  
namespace MyControls  
{  
  public partial class MyControl1 : Grid  
  {  
    //...  
  }  
  public class MyControlEventArgs : EventArgs  
  {  
    //...  
  }  
}  
```  
  
 第一個類別是`MyControl1`部分類別, 其中包含的程式碼會執行 mycontrol1.xaml 中定義的[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]功能。 剖析 mycontrol1.xaml 時, [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]會將轉換成相同的部分類別, 併合並兩個部分類別來形成編譯的控制項。 因此，程式碼後置檔案中的類別名稱必須符合指派給 MyControl1.xaml 的類別名稱，而且必須繼承自控制項的根項目。 第二個類別`MyControlEventArgs`是用來將資料傳回給主控制項的事件引數類別。  
  
 開啟 MyControl1.xaml.cs。 變更現有的類別宣告, 使其具有下列名稱, 並繼承自<xref:System.Windows.Controls.Grid>。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#21](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml.cs#21)]  
  
#### <a name="initializing-the-control"></a>初始化控制項  
 下例程式碼實作數個基本工作︰  
  
- 宣告私用事件`OnButtonClick`和其相關聯的`MyControlEventHandler`委派。  
  
- 建立可儲存使用者資料的數個私用全域變數。 這項資料是透過對應的屬性所公開。  
  
- 針對控制項的<xref:System.Windows.FrameworkElement.Loaded>事件`Init`, 執行處理常式。 此處理常式會初始化全域變數，方法是將 MyControl1.xaml 中所定義的值指派給它們。 若要這樣做, 它會<xref:System.Windows.FrameworkElement.Name%2A>使用指派給一般<xref:System.Windows.Controls.TextBlock> `nameLabel`專案的, 來存取該專案的屬性設定。  
  
 刪除現有的函式, 並將下列程式碼`MyControl1`新增至您的類別。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#11](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml.cs#11)]  
  
#### <a name="handling-the-buttons-click-events"></a>處理 Buttons 的 Click 事件  
 使用者可以按一下 [**確定]** 按鈕或 [**取消**] 按鈕, 來指出資料輸入工作已完成。 這兩個按鈕都<xref:System.Windows.Controls.Primitives.ButtonBase.Click>使用相同的`ButtonClicked`事件處理常式。 這兩個按鈕都有`btnOK`名稱`btnCancel`或, 可讓處理常式藉由`sender`檢查引數的值來判斷按一下的按鈕。 此處理常式會執行下列動作︰  
  
- 建立物件, 其中包含<xref:System.Windows.Controls.TextBox>來自元素的資料。 `MyControlEventArgs`  
  
- 如果使用者按一下 [**取消**] 按鈕, 則會`MyControlEventArgs`將物件`IsOK`的屬性`false`設定為。  
  
- `OnButtonClick`引發事件, 向主機表示使用者已完成, 並傳回所收集的資料。  
  
 在`MyControl1` 方法`Init`後面, 將下列程式碼新增至您的類別。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#12](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml.cs#12)]  
  
#### <a name="creating-properties"></a>建立屬性  
 類別的其餘部分只會公開對應至先前所討論之全域變數的屬性。 屬性變更時，set 存取子會變更對應的項目屬性以及更新基礎全域變數來修改控制項的外觀。  
  
 將下列程式碼新增至`MyControl1`您的類別。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#13](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml.cs#13)]  
  
#### <a name="sending-the-data-back-to-the-host"></a>將資料傳回主應用程式  
 檔案中的最後一個元件是`MyControlEventArgs`類別, 用來將收集的資料傳送回主機。  
  
 將下列程式碼新增至`MyControls`您的命名空間。 此實作十分簡單，未來不會進行討論。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#14](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/MyControls/Page1.xaml.cs#14)]  
  
 建置方案。 組置將會產生名為 MyControls.dll 的 DLL。  
  
<a name="winforms_host_section"></a>   
## <a name="implementing-the-windows-forms-host-application"></a>實作 Windows Forms 主應用程式  
 Windows Forms 主應用程式會使用<xref:System.Windows.Forms.Integration.ElementHost>物件來[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]裝載複合控制項。 應用程式會處理`OnButtonClick`事件, 以接收來自複合控制項的資料。 應用程式也會有一組選項按鈕，可用來修改控制項的外觀。 下圖顯示應用程式。  

下圖顯示裝載于 Windows Forms 應用程式中的 WPF 複合控制項」  

 ![Scteenshot, 其中顯示裝載 Avalon 控制項的 Windows Form。](./media/walkthrough-hosting-a-wpf-composite-control-in-windows-forms/windows-form-hosting-avalon-control.png)  
  
### <a name="creating-the-project"></a>建立專案  
 啟動專案：  
  
1. 啟動[!INCLUDE[TLA2#tla_visualstu](../../../../includes/tla2sharptla-visualstu-md.md)], 然後開啟 [**新增專案**] 對話方塊。  
  
2. 在 [ C#視覺效果] 和 [Windows 分類] 中, 選取 [ **Windows Forms 應用程式**] 範本。  
  
3. 將新專案命名為 `WFHost`。  
  
4. 針對位置，指定包含 MyControls 專案的相同最上層資料夾。  
  
5. 按一下 \[確定\] 來建立專案。  
  
 您也需要加入包含`MyControl1`和其他元件之 DLL 的參考。  
  
1. 以滑鼠右鍵按一下方案總管中的專案名稱, 然後選取 [**新增參考**]。  
  
2. 按一下 [**流覽**] 索引標籤, 然後流覽至包含 mycontrols.dll 的資料夾。 在此逐步解說中，這個資料夾是 MyControls\bin\Debug。  
  
3. 選取 [Mycontrols.dll], 然後按一下 **[確定]** 。  
  
4. 加入下列組件的參考。  
  
    - PresentationCore  
  
    - PresentationFramework  
  
    - System.Xaml  
  
    - WindowsBase  
  
    - WindowsFormsIntegration  
  
### <a name="implementing-the-user-interface-for-the-application"></a>實作應用程式的使用者介面  
 Windows Forms 應用程式的 UI 包含數個控制項，可與 WPF 複合控制項互動。  
  
1. 在 Windows Forms 設計工具中，開啟 Form1。  
  
2. 放大表單，以容納控制項。  
  
3. 在表單的右上角, 加入<xref:System.Windows.Forms.Panel?displayProperty=nameWithType>控制項以[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]保存複合控制項。  
  
4. 將下列<xref:System.Windows.Forms.GroupBox?displayProperty=nameWithType>控制項新增至表單。  
  
    |名稱|文字|  
    |----------|----------|  
    |groupBox1|背景色彩|  
    |groupBox2|前景色彩|  
    |groupBox3|字型大小|  
    |groupBox4|字型家族|  
    |groupBox5|字型樣式|  
    |groupBox6|字型粗細|  
    |groupBox7|來自控制項的資料|  
  
5. 將下列<xref:System.Windows.Forms.RadioButton?displayProperty=nameWithType>控制項新增<xref:System.Windows.Forms.GroupBox?displayProperty=nameWithType>至控制項。  
  
    |GroupBox|名稱|文字|  
    |--------------|----------|----------|  
    |groupBox1|radioBackgroundOriginal|原始|  
    |groupBox1|radioBackgroundLightGreen|LightGreen|  
    |groupBox1|radioBackgroundLightSalmon|LightSalmon|  
    |groupBox2|radioForegroundOriginal|原始|  
    |groupBox2|radioForegroundRed|紅色|  
    |groupBox2|radioForegroundYellow|黃色|  
    |groupBox3|radioSizeOriginal|原始|  
    |groupBox3|radioSizeTen|10|  
    |groupBox3|radioSizeTwelve|12|  
    |groupBox4|radioFamilyOriginal|原始|  
    |groupBox4|radioFamilyTimes|Tw Cen MT Condensed|  
    |groupBox4|radioFamilyWingDings|WingDings|  
    |groupBox5|radioStyleOriginal|一般|  
    |groupBox5|radioStyleItalic|斜體|  
    |groupBox6|radioWeightOriginal|原始|  
    |groupBox6|radioWeightBold|Bold|  
  
6. 將下列<xref:System.Windows.Forms.Label?displayProperty=nameWithType>控制項加入至最後一個<xref:System.Windows.Forms.GroupBox?displayProperty=nameWithType>。 這些控制項會顯示[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]複合控制項所傳回的資料。  
  
    |GroupBox|名稱|文字|  
    |--------------|----------|----------|  
    |groupBox7|lblName|名稱：|  
    |groupBox7|lblAddress|街道地址：|  
    |groupBox7|lblCity|城市：|  
    |groupBox7|lblState|狀態:|  
    |groupBox7|lblZip|郵遞區號︰|  
  
### <a name="initializing-the-form"></a>初始化表單  
 您通常會在表單的<xref:System.Windows.Forms.Form.Load>事件處理常式中, 執行裝載程式碼。 下列程式碼顯示<xref:System.Windows.Forms.Form.Load>事件處理常式、 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]複合控制項的<xref:System.Windows.FrameworkElement.Loaded>事件處理常式, 以及稍後使用的數個全域變數的宣告。  
  
 在 Windows Form 設計工具中, 按兩下表單以建立<xref:System.Windows.Forms.Form.Load>事件處理常式。 在 Form1.cs 頂端, 加入下列`using`語句。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#10](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/WFHost/Form1.cs#10)]  
  
 使用下列程式碼取代現有`Form1`類別的內容。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#2](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/WFHost/Form1.cs#2)]  
  
 上述`Form1_Load`程式碼中的方法會顯示[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]裝載控制項的一般程式:  
  
1. 建立新<xref:System.Windows.Forms.Integration.ElementHost>的物件。  
  
2. 將控制項的<xref:System.Windows.Forms.Control.Dock%2A>屬性設為<xref:System.Windows.Forms.DockStyle.Fill?displayProperty=nameWithType>。  
  
3. 將控制項新增<xref:System.Windows.Forms.Panel>至控制項的<xref:System.Windows.Forms.Control.Controls%2A>集合。 <xref:System.Windows.Forms.Integration.ElementHost>  
  
4. 建立[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]控制項的實例。  
  
5. 藉由將控制項指派給<xref:System.Windows.Forms.Integration.ElementHost>控制項的<xref:System.Windows.Forms.Integration.ElementHost.Child%2A>屬性, 將複合控制項裝載在表單上。  
  
 `Form1_Load`方法中剩餘的兩行會將處理常式附加至兩個控制項事件:  
  
- `OnButtonClick`是當使用者按一下 **[確定]** 或 [**取消**] 按鈕時, 由複合控制項引發的自訂事件。 您可以處理事件以取得使用者回應，以及收集使用者所指定的任何資料。  
  
- <xref:System.Windows.FrameworkElement.Loaded>是由[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]控制項完全載入時所引發的標準事件。 因為此範例需要使用控制項中的屬性來初始化數個全域變數，所以在這裡使用這個事件。 在表單<xref:System.Windows.Forms.Form.Load>的事件發生時, 控制項並未完全載入, 而且這些值仍會設定為`null`。 您必須等到控制項的<xref:System.Windows.FrameworkElement.Loaded>事件發生後, 才能存取這些屬性。  
  
 <xref:System.Windows.FrameworkElement.Loaded>事件處理常式會顯示在上述程式碼中。 下`OnButtonClick`一節將討論此處理程式。  
  
### <a name="handling-onbuttonclick"></a>處理 OnButtonClick  
 當`OnButtonClick`使用者按一下 [**確定]** 或 [**取消**] 按鈕時, 就會發生此事件。  
  
 事件處理常式會檢查事件引數`IsOK`的欄位, 以判斷按一下哪一個按鈕。 資料變數會對應至稍<xref:System.Windows.Forms.Label>早所討論的控制項。 `lbl` 如果使用者按一下 [**確定]** 按鈕, 則會將控制項<xref:System.Windows.Controls.TextBox>控制項的資料指派給對應<xref:System.Windows.Forms.Label>的控制項。 如果使用者按一下 [**取消**], <xref:System.Windows.Forms.Label.Text%2A>則值會設定為預設字串。  
  
 將下列按鈕 click 事件處理常式程式碼加入`Form1`至類別。  
  
 [!code-csharp[WindowsFormsHostingWpfControl#3](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/WFHost/Form1.cs#3)]  
  
 建置並執行應用程式。 在 WPF 複合控制項中新增一些文字, 然後按一下 **[確定]** 。 文字會顯示在標籤中。 此時，尚未新增程式碼來處理選項按鈕。  
  
### <a name="modifying-the-appearance-of-the-control"></a>修改控制項的外觀  
 窗<xref:System.Windows.Forms.RadioButton>體上的控制項可讓使用者[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]變更複合控制項的前景和背景色彩, 以及數個字型屬性。 背景色彩是由<xref:System.Windows.Forms.Integration.ElementHost>物件所公開。 其餘的屬性會公開為控制項的自訂屬性。  
  
 按兩下表單上<xref:System.Windows.Forms.RadioButton>的每個控制項, 以<xref:System.Windows.Forms.RadioButton.CheckedChanged>建立事件處理常式。 以下列程式碼取代事件處理常式。<xref:System.Windows.Forms.RadioButton.CheckedChanged>  
  
 [!code-csharp[WindowsFormsHostingWpfControl#4](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsFormsHostingWpfControl/CSharp/WFHost/Form1.cs#4)]  
  
 建置並執行應用程式。 按一下不同的選項按鈕，以查看在 WPF 複合控制項上的效果。  
  
## <a name="see-also"></a>另請參閱

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [在 Visual Studio 中設計 XAML](/visualstudio/designers/designing-xaml-in-visual-studio)
- [逐步解說：在 WPF 中裝載 Windows Forms 複合控制項](walkthrough-hosting-a-windows-forms-composite-control-in-wpf.md)
- [逐步解說：在 Windows Forms 中裝載立體 WPF 複合控制項](walkthrough-hosting-a-3-d-wpf-composite-control-in-windows-forms.md)
