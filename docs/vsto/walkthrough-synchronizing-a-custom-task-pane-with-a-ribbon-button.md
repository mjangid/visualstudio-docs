---
title: "Walkthrough: Synchronizing a Custom Task Pane with a Ribbon Button | Microsoft Docs"
ms.custom: ""
ms.date: "02/02/2017"
ms.prod: "visual-studio-dev14"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "office-development"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
helpviewer_keywords: 
  - "custom task panes [Office development in Visual Studio], showing and hiding"
  - "showing custom task panes"
  - "Ribbon [Office development in Visual Studio], custom task panes"
  - "toggle buttons [Office development in Visual Studio]"
  - "custom task panes [Office development in Visual Studio], synchronizing with Ribbon button"
  - "user interfaces [Office development in Visual Studio], custom task panes"
  - "synchronization [Office development in Visual Studio], custom task panes"
  - "task panes [Office development in Visual Studio], showing and hiding"
  - "custom task panes [Office development in Visual Studio], creating"
  - "hiding custom task panes"
  - "task panes [Office development in Visual Studio], creating"
  - "task panes [Office development in Visual Studio], synchronizing with Ribbon button"
ms.assetid: 00ce8b1e-1370-42f2-9dc9-609cada392f1
caps.latest.revision: 38
author: "kempb"
ms.author: "kempb"
manager: "ghogen"
---
# Walkthrough: Synchronizing a Custom Task Pane with a Ribbon Button
  This walkthrough demonstrates how to create a custom task pane that users can hide or display by clicking a toggle button on the Ribbon. You should always create a user interface (UI) element, such as a button, that users can click to display or hide your custom task pane, because Microsoft Office applications do not provide a default way for users to show or hide custom task panes.  
  
 [!INCLUDE[appliesto_olkallapp](../vsto/includes/appliesto-olkallapp-md.md)]  
  
 Although this walkthrough uses Excel specifically, the concepts demonstrated by the walkthrough are applicable to any applications that are listed above.  
  
 This walkthrough illustrates the following tasks:  
  
-   Designing the UI of the custom task pane.  
  
-   Adding a toggle button to the Ribbon.  
  
-   Synchronizing the toggle button with the custom task pane.  
  
> [!NOTE]  
>  Your computer might show different names or locations for some of the Visual Studio user interface elements in the following instructions. The Visual Studio edition that you have and the settings that you use determine these elements. For more information, see [Personalize the Visual Studio IDE](../ide/personalizing-the-visual-studio-ide.md).  
  
## Prerequisites  
 You need the following components to complete this walkthrough:  
  
-   [!INCLUDE[vsto_vsprereq](../vsto/includes/vsto-vsprereq-md.md)]  
  
-   Microsoft Excel or Microsoft [!INCLUDE[Excel_15_short](../vsto/includes/excel-15-short-md.md)].  
  
## Creating the Add-in Project  
 In this step, you will create an VSTO Add-in project for Excel.  
  
#### To create a new project  
  
1.  Create an Excel Add-in project with the name **SynchronizeTaskPaneAndRibbon**, using the Excel Add-in project template. For more information, see [How to: Create Office Projects in Visual Studio](../vsto/how-to-create-office-projects-in-visual-studio.md).  
  
     [!INCLUDE[vsprvs](../sharepoint/includes/vsprvs-md.md)] opens the **ThisAddIn.cs** or **ThisAddIn.vb** code file and adds the **SynchronizeTaskPaneAndRibbon** project to **Solution Explorer**.  
  
## Adding a Toggle Button to the Ribbon  
 One of the Office application design guidelines is that users should always have control of the Office application UI. To enable users to control the custom task pane, you can add a Ribbon toggle button that shows and hides the task pane. To create a toggle button, add a **Ribbon (Visual Designer)** item to the project. The designer helps you add and position controls, set control properties, and handle control events. For more information, see [Ribbon Designer](../vsto/ribbon-designer.md).  
  
#### To add a toggle button to the Ribbon  
  
1.  On the **Project** menu, click **Add New Item**.  
  
2.  In the **Add New Item** dialog box, select **Ribbon (Visual Designer)**.  
  
3.  Change the name of the new Ribbon to **ManageTaskPaneRibbon**, and click **Add**.  
  
     The **ManageTaskPaneRibbon.cs** or **ManageTaskPaneRibbon.vb** file opens in the Ribbon Designer and displays a default tab and group.  
  
4.  In the Ribbon Designer, click **group1**.  
  
5.  In the **Properties** window, set the **Label** property to **Task Pane Manager**.  
  
6.  From the **Office Ribbon Controls** tab of the **Toolbox**, drag a **ToggleButton** onto the **Task Pane Manager** group.  
  
7.  Click **toggleButton1**.  
  
8.  In the **Properties** window, set the **Label** property to **Show Task Pane**.  
  
## Designing the User Interface of the Custom Task Pane  
 There is no visual designer for custom task panes, but you can design a user control with the layout you want. Later in this walkthrough, you will add the user control to the custom task pane.  
  
#### To design the user interface of the custom task pane  
  
1.  On the **Project** menu, click **Add User Control**.  
  
2.  In the **Add New Item** dialog box, change the name of the user control to **TaskPaneControl**, and click **Add**.  
  
     The user control opens in the designer.  
  
3.  From the **Common Controls** tab of the **Toolbox**, drag a **TextBox** control to the user control.  
  
## Creating the Custom Task Pane  
 To create the custom task pane when the VSTO Add-in starts, add the user control to the task pane in the <xref:Microsoft.Office.Tools.AddIn.Startup> event handler of the VSTO Add-in. By default, the custom task pane will not be visible. Later in this walkthrough, you will add code that will display or hide the task pane when the user clicks the toggle button you added to the Ribbon.  
  
#### To create the custom task pane  
  
1.  In **Solution Explorer**, expand **Excel**.  
  
2.  Right-click **ThisAddIn.cs** or **ThisAddIn.vb** and click **View Code**.  
  
3.  Add the following code to the `ThisAddIn` class. This code declares an instance of `TaskPaneControl` as a member of `ThisAddIn`.  
  
     [!code-cs[Trin_TaskPaneRibbonSynchronize#1](../vsto/codesnippet/CSharp/Trin_TaskPaneRibbonSynchronize/ThisAddIn.cs#1)]
     [!code-vb[Trin_TaskPaneRibbonSynchronize#1](../vsto/codesnippet/VisualBasic/Trin_TaskPaneRibbonSynchronize/ThisAddIn.vb#1)]  
  
4.  Replace the `ThisAddIn_Startup` event handler with the following code. This code adds the `TaskPaneControl` object to the `CustomTaskPanes` field, but it does not display the custom task pane (by default, the <xref:Microsoft.Office.Tools.CustomTaskPane.Visible%2A> property of the <xref:Microsoft.Office.Tools.CustomTaskPane> class is **false**). The Visual C# code also attaches an event handler to the <xref:Microsoft.Office.Tools.CustomTaskPane.VisibleChanged> event.  
  
     [!code-cs[Trin_TaskPaneRibbonSynchronize#2](../vsto/codesnippet/CSharp/Trin_TaskPaneRibbonSynchronize/ThisAddIn.cs#2)]
     [!code-vb[Trin_TaskPaneRibbonSynchronize#2](../vsto/codesnippet/VisualBasic/Trin_TaskPaneRibbonSynchronize/ThisAddIn.vb#2)]  
  
5.  Add the following method to the `ThisAddIn` class. This method handles the <xref:Microsoft.Office.Tools.CustomTaskPane.VisibleChanged> event. When the user closes the task pane by clicking the **Close** button (X), this method updates the state of the toggle button on the Ribbon.  
  
     [!code-cs[Trin_TaskPaneRibbonSynchronize#3](../vsto/codesnippet/CSharp/Trin_TaskPaneRibbonSynchronize/ThisAddIn.cs#3)]
     [!code-vb[Trin_TaskPaneRibbonSynchronize#3](../vsto/codesnippet/VisualBasic/Trin_TaskPaneRibbonSynchronize/ThisAddIn.vb#3)]  
  
6.  Add the following property to the `ThisAddIn` class. This property exposes the private `myCustomTaskPane1` object to other classes. Later in this walkthrough, you will add code to the `MyRibbon` class that uses this property.  
  
     [!code-cs[Trin_TaskPaneRibbonSynchronize#4](../vsto/codesnippet/CSharp/Trin_TaskPaneRibbonSynchronize/ThisAddIn.cs#4)]
     [!code-vb[Trin_TaskPaneRibbonSynchronize#4](../vsto/codesnippet/VisualBasic/Trin_TaskPaneRibbonSynchronize/ThisAddIn.vb#4)]  
  
## Hiding and Showing the Custom Task Pane by Using the Toggle Button  
 The last step is to add code that displays or hides the custom task pane when the user clicks the toggle button on the Ribbon.  
  
#### To display and hide the custom task pane by using the toggle button  
  
1.  In the Ribbon Designer, double-click the **Show Task Pane** toggle button.  
  
     Visual Studio automatically generates an event handler named `toggleButton1_Click`, which handles the <xref:Microsoft.Office.Tools.Ribbon.RibbonToggleButton.Click> event of the toggle button. Visual Studio also opens the **MyRibbon.cs** or **MyRibbon.vb** file in the Code Editor.  
  
2.  Replace the `toggleButton1_Click` event handler with the following code. When the user clicks the toggle button, this code displays or hides the custom task pane, depending on whether the toggle button is pressed or not pressed.  
  
     [!code-vb[Trin_TaskPaneRibbonSynchronize#5](../vsto/codesnippet/VisualBasic/Trin_TaskPaneRibbonSynchronize/ManageTaskPaneRibbon.vb#5)]
     [!code-cs[Trin_TaskPaneRibbonSynchronize#5](../vsto/codesnippet/CSharp/Trin_TaskPaneRibbonSynchronize/ManageTaskPaneRibbon.cs#5)]  
  
## Testing the Add-In  
 When you run the project, Excel opens without displaying the custom task pane. Click the toggle button on the Ribbon to test the code.  
  
#### To test your VSTO Add-in  
  
1.  Press F5 to run your project.  
  
     Confirm that Excel opens, and the **Add-Ins** tab appears on the Ribbon.  
  
2.  Click the **Add-Ins** tab on the Ribbon.  
  
3.  In the **Task Pane Manager** group, click the **Show Task Pane** toggle button.  
  
     Verify that the task pane is alternately displayed and hidden when you click the toggle button.  
  
4.  When the task pane is visible, click the **Close** button (X) in the corner of the task pane.  
  
     Verify that the toggle button appears to be not pressed.  
  
## Next Steps  
 You can learn more about how to create custom task panes from these topics:  
  
-   Create a custom task pane in an VSTO Add-in for a different application. For more information about the applications that support custom task panes, see [Custom Task Panes](../vsto/custom-task-panes.md).  
  
-   Automate an application from a custom task pane. For more information, see [Walkthrough: Automating an Application from a Custom Task Pane](../vsto/walkthrough-automating-an-application-from-a-custom-task-pane.md).  
  
-   Create a custom task pane for every e-mail message that is opened in Outlook. For more information, see [Walkthrough: Displaying Custom Task Panes with E-Mail Messages in Outlook](../vsto/walkthrough-displaying-custom-task-panes-with-e-mail-messages-in-outlook.md).  
  
## See Also  
 [Custom Task Panes](../vsto/custom-task-panes.md)   
 [How to: Add a Custom Task Pane to an Application](../vsto/how-to-add-a-custom-task-pane-to-an-application.md)   
 [Walkthrough: Automating an Application from a Custom Task Pane](../vsto/walkthrough-automating-an-application-from-a-custom-task-pane.md)   
 [Walkthrough: Displaying Custom Task Panes with E-Mail Messages in Outlook](../vsto/walkthrough-displaying-custom-task-panes-with-e-mail-messages-in-outlook.md)   
 [Ribbon Overview](../vsto/ribbon-overview.md)  
  
  