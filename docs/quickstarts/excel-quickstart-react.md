---
title: Build an Excel task pane add-in using React
description: 
ms.date: 05/02/2019
ms.prod: excel
localization_priority: Priority
---

# Build an Excel task pane add-in using React

In this article, you'll walk through the process of building an Excel task pane add-in using React and the Excel JavaScript API.

## Prerequisites

[!include[Yeoman generator prerequisites](../includes/quickstart-yo-prerequisites.md)]

## Create the add-in project

[!include[note about Yeoman generator bug](../includes/note-yeoman-generator-bug-201908.md)]

Use the Yeoman generator to create an Excel add-in project. Run the following command and then answer the prompts as follows:

```command&nbsp;line
yo office
```

- **Choose a project type:** `Office Add-in Task Pane project using React framework`
- **Choose a script type:** `TypeScript`
- **What do you want to name your add-in?** `my-office-add-in`
- **Which Office client application would you like to support?** `Excel`

After you complete the wizard, the generator creates the project and installs supporting Node components.

## Explore the project

The add-in project that you've created with the Yeoman generator contains sample code for a very basic task pane add-in. If you'd like to explore the key components of your add-in project, open the project in your code editor and review the files listed below. When you're ready to try out your add-in, proceed to the next section.

- The **manifest.xml** file in the root directory of the project defines the settings and capabilities of the add-in.
- The **./src/taskpane/taskpane.html** file defines the HTML framework of the task pane, and the files within the **./src/taskpane/components** folder define the various parts of the task pane UI.
- The **./src/taskpane/taskpane.css** file contains the CSS that's applied to content in the task pane.
- The **./src/taskpane/components/App.tsx** file contains the Office JavaScript API code that facilitates interaction between the task pane and Excel.

## Try it out

1. Navigate to the root folder of the project.

    ```command&nbsp;line
    cd "my-office-add-in"
    ```

2. [!include[Start server section](../includes/quickstart-yo-start-server-excel.md)] 

3. In Excel, choose the **Home** tab, and then choose the **Show Taskpane** button in the ribbon to open the add-in task pane.

    ![Excel add-in button](../images/excel-quickstart-addin-3b.png)

4. Select any range of cells in the worksheet.

5. At the bottom of the task pane, choose the **Run** link to set the color of the selected range to yellow.

    ![Excel add-in](../images/excel-quickstart-addin-3c.png)

## Next steps

Congratulations, you've successfully created an Excel task pane add-in using React! Next, learn more about the capabilities of an Excel add-in and build a more complex add-in by following along with the Excel add-in tutorial.

> [!div class="nextstepaction"]
> [Excel add-in tutorial](../tutorials/excel-tutorial.md)

## See also

* [Excel add-in tutorial](../tutorials/excel-tutorial-create-table.md)
* [Fundamental programming concepts with the Excel JavaScript API](../excel/excel-add-ins-core-concepts.md)
* [Excel add-in code samples](https://developer.microsoft.com/office/gallery/?filterBy=Samples,Excel)
* [Excel JavaScript API reference](/office/dev/add-ins/reference/overview/excel-add-ins-reference-overview)
