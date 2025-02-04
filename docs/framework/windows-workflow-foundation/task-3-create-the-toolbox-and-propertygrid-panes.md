---
title: 工作 3：建立工具箱與 PropertyGrid 窗格
ms.date: 03/30/2017
ms.assetid: 72c1546a-eed5-4f0f-a616-719a163414f4
ms.openlocfilehash: 6339969c52a5c4eedfb0e89eebdc982ca3fe6686
ms.sourcegitcommit: 005980b14629dfc193ff6cdc040800bc75e0a5a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2019
ms.locfileid: "70988704"
---
# <a name="task-3-create-the-toolbox-and-propertygrid-panes"></a>工作 3：建立工具箱與 PropertyGrid 窗格
在這項工作中，您將建立 [**工具箱**] 和 [ **PropertyGrid** ] 窗格， [!INCLUDE[wfd1](../../../includes/wfd1-md.md)]並將其新增至重新裝載。  
  
 如需參考，在完成重新裝載中的三個工作之後，應該在 MainWindow.xaml.cs 檔案中的程式碼，本主題的結尾會提供[工作流程設計工具](rehosting-the-workflow-designer.md)的主題系列。  
  
### <a name="to-create-the-toolbox-and-add-it-to-the-grid"></a>若要建立工具箱，並將它加入至方格  
  
1. 依照工作2：下列程式中[所述的步驟，開啟您取得的 HostingApplication 專案。裝載工作流程設計工具](task-2-host-the-workflow-designer.md)。  
  
2. 在 [**方案總管**] 窗格中，以滑鼠右鍵按一下 [mainwindow.xaml] 檔案，然後選取 [ **View Code**]。  
  
3. `MainWindow` <xref:System.Activities.Presentation.Toolbox.ToolboxControl> <xref:System.Activities.Statements.Sequence> <xref:System.Activities.Statements.Assign>將方法新增至建立的類別，將新的 [工具箱] 分類加入 [工具箱]，並將和活動類型指派給該分類。 `GetToolboxControl`  
  
    ```csharp  
    private ToolboxControl GetToolboxControl()  
    {  
        // Create the ToolBoxControl.  
        ToolboxControl ctrl = new ToolboxControl();  
  
        // Create a category.  
        ToolboxCategory category = new ToolboxCategory("category1");  
  
        // Create Toolbox items.  
        ToolboxItemWrapper tool1 =   
            new ToolboxItemWrapper("System.Activities.Statements.Assign",   
            typeof(Assign).Assembly.FullName, null, "Assign");  
  
        ToolboxItemWrapper tool2 = new ToolboxItemWrapper("System.Activities.Statements.Sequence",   
            typeof(Sequence).Assembly.FullName, null, "Sequence");  
  
        // Add the Toolbox items to the category.  
        category.Add(tool1);  
        category.Add(tool2);  
  
        // Add the category to the ToolBox control.  
        ctrl.Categories.Add(category);  
        return ctrl;  
    }  
    ```  
  
4. 將私`AddToolbox`用方法`MainWindow`加入類別，將 [**工具箱**] 放在方格的左欄中。  
  
    ```csharp  
    private void AddToolBox()  
    {  
        ToolboxControl tc = GetToolboxControl();  
        Grid.SetColumn(tc, 0);  
        grid1.Children.Add(tc);  
    }  
    ```  
  
5. 在 `AddToolBox` 類別建構函式中加入 `MainWindow()` 方法的呼叫，如下列程式碼所示。  
  
    ```csharp  
    public MainWindow()  
    {  
        InitializeComponent();  
        this.RegisterMetadata();  
        this.AddDesigner();  
  
        this.AddToolBox();  
    }  
    ```  
  
6. 按 F5 以建置及執行您的方案。 包含<xref:System.Activities.Statements.Assign> 和<xref:System.Activities.Statements.Sequence>活動的 [**工具箱**] 應該會顯示。  
  
### <a name="to-create-the-propertygrid"></a>若要建立 PropertyGrid  
  
1. 在 [**方案總管**] 窗格中，以滑鼠右鍵按一下 [mainwindow.xaml] 檔案，然後選取 [ **View Code**]。  
  
2. 將方法新增至類別，以將 [PropertyGrid] 窗格放在方格的最右邊資料行中。 `MainWindow` `AddPropertyInspector`  
  
    ```csharp  
    private void AddPropertyInspector()  
    {  
        Grid.SetColumn(wd.PropertyInspectorView, 2);  
        grid1.Children.Add(wd.PropertyInspectorView);              
    }  
    ```  
  
3. 在 `AddPropertyInspector` 類別建構函式中加入 `MainWindow()` 方法的呼叫，如下列程式碼所示。  
  
    ```csharp  
    public MainWindow()  
    {  
        InitializeComponent();  
        this.RegisterMetadata();  
        this.AddDesigner();  
        this.AddToolBox();  
  
        this.AddPropertyInspector();   
    }  
    ```  
  
4. 按 F5 以建置及執行方案。 [**工具箱**]、[工作流程設計畫布] 和 [ **PropertyGrid** ] 窗格都應該顯示出來， <xref:System.Activities.Statements.Assign>而且當您<xref:System.Activities.Statements.Sequence>將活動或活動拖曳到 [設計] 畫布上時，屬性方格應該會更新，視反白顯示的活動。  
  
## <a name="example"></a>範例  
 MainWindow.xaml.cs 檔案現在應該會包含下列程式碼。  
  
```csharp  
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.Windows;  
using System.Windows.Controls;  
using System.Windows.Data;  
using System.Windows.Documents;  
using System.Windows.Input;  
using System.Windows.Media;  
using System.Windows.Media.Imaging;  
using System.Windows.Navigation;  
using System.Windows.Shapes;  
//dlls added  
using System.Activities;  
using System.Activities.Core.Presentation;  
using System.Activities.Presentation;  
using System.Activities.Presentation.Metadata;  
using System.Activities.Presentation.Toolbox;  
using System.Activities.Statements;  
using System.ComponentModel;  
  
namespace HostingApplication  
{  
    /// <summary>  
    /// Interaction logic for MainWindow.xaml  
    /// </summary>  
    public partial class MainWindow : Window  
    {  
        private WorkflowDesigner wd;  
  
        public MainWindow()  
        {  
            InitializeComponent();  
            RegisterMetadata();  
            AddDesigner();  
            this.AddToolBox();  
            this.AddPropertyInspector();  
        }  
  
        private void AddDesigner()  
        {  
            //Create an instance of WorkflowDesigner class.  
            this.wd = new WorkflowDesigner();  
  
            //Place the designer canvas in the middle column of the grid.  
            Grid.SetColumn(this.wd.View, 1);  
  
            //Load a new Sequence as default.  
            this.wd.Load(new Sequence());  
  
            //Add the designer canvas to the grid.  
            grid1.Children.Add(this.wd.View);  
        }  
  
        private void RegisterMetadata()  
        {  
            DesignerMetadata dm = new DesignerMetadata();  
            dm.Register();  
        }  
  
        private ToolboxControl GetToolboxControl()  
        {  
            // Create the ToolBoxControl.  
            ToolboxControl ctrl = new ToolboxControl();  
  
            // Create a category.  
            ToolboxCategory category = new ToolboxCategory("category1");  
  
            // Create Toolbox items.  
            ToolboxItemWrapper tool1 =  
                new ToolboxItemWrapper("System.Activities.Statements.Assign",  
                typeof(Assign).Assembly.FullName, null, "Assign");  
  
            ToolboxItemWrapper tool2 = new ToolboxItemWrapper("System.Activities.Statements.Sequence",  
                typeof(Sequence).Assembly.FullName, null, "Sequence");  
  
            // Add the Toolbox items to the category.  
            category.Add(tool1);  
            category.Add(tool2);  
  
            // Add the category to the ToolBox control.  
            ctrl.Categories.Add(category);  
            return ctrl;  
        }  
  
        private void AddToolBox()  
        {  
            ToolboxControl tc = GetToolboxControl();  
            Grid.SetColumn(tc, 0);  
            grid1.Children.Add(tc);  
        }  
  
        private void AddPropertyInspector()  
        {  
            Grid.SetColumn(wd.PropertyInspectorView, 2);  
            grid1.Children.Add(wd.PropertyInspectorView);  
        }  
  
    }  
}  
```  
  
## <a name="see-also"></a>另請參閱

- [重新裝載工作流程設計工具](rehosting-the-workflow-designer.md)
- [工作1：建立新的 Windows Presentation Foundation 應用程式](task-1-create-a-new-wpf-app.md)
- [工作2：裝載工作流程設計工具](task-2-host-the-workflow-designer.md)
