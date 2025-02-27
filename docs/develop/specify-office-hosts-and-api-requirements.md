---
title: Specify Office hosts and API requirements
description: ''
ms.date: 08/14/2019
localization_priority: Priority
---

# Specify Office hosts and API requirements

Your Office Add-in might depend on a specific Office host, a requirement set, an API member, or a version of the API in order to work as expected. For example, your add-in might:

- Run in a single Office application (e.g., Word or Excel), or several applications.

- Make use of JavaScript APIs that are only available in some versions of Office. For example, you might use the Excel JavaScript APIs in an add-in that runs in Excel 2016.

- Run only in versions of Office that support API members that your add-in uses.

This article helps you understand which options you should choose to ensure that your add-in works as expected and reaches the broadest audience possible.

> [!NOTE]
> For a high-level view of where Office Add-ins are currently supported, see the [Office Add-in host and platform availability](../overview/office-add-in-availability.md) page.

The following table lists core concepts discussed throughout this article.

|**Concept**|**Description**|
|:-----|:-----|
|Office application, Office host application, Office host, or host|The Office application used to run your add-in. For example, Word, Excel, and so on.|
|Platform|Where the Office host runs, such as in a browser or on an iPad.|
|Requirement set|A named group of related API members. Add-ins use requirement sets to determine whether the Office host supports API members used by your add-in. It's easier to test for the support of a requirement set than for the support of individual API members. Requirement set support varies by Office host and the version of the Office host. <br >Requirement sets are specified in the manifest file. When you specify requirement sets in the manifest, you set the minimum level of API support that the Office host must provide in order to run your add-in. Office hosts that don't support requirement sets specified in the manifest can't run your add-in, and your add-in won't display in <span class="ui">My Add-ins</span>. This restricts where your add-in is available. In code using runtime checks. For the complete list of requirement sets, see [Office Add-in requirement sets](/office/dev/add-ins/reference/requirement-sets/office-add-in-requirement-sets).|
|Runtime check|A test that is performed at runtime to determine whether the Office host running your add-in supports requirement sets or methods used by your add-in. To perform a runtime check, you use an  **if** statement with the **isSetSupported** method, the requirement sets, or the method names that aren't part of a requirement set. Use runtime checks to ensure that your add-in reaches the broadest number of customers. Unlike requirement sets, runtime checks don't specify the minimum level of API support that the Office host must provide for your add-in to run. Instead, you use the  **if** statement to determine whether an API member is supported. If it is, you can provide additional functionality in your add-in. Your add-in will always display in **My Add-ins** when you use runtime checks.|

## Before you begin

Your add-in must use the most current version of the add-in manifest schema. If you use runtime checks in your add-in, ensure that you use the latest JavaScript API for Office (office.js) library.

### Specify the latest add-in manifest schema

Your add-in's manifest must use version 1.1 of the add-in manifest schema. Set the  **OfficeApp** element in your add-in manifest as follows.

```XML
<OfficeApp xmlns="http://schemas.microsoft.com/office/appforoffice/1.1" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:type="TaskPaneApp">
```

### Specify the latest JavaScript API for Office library

If you use runtime checks, reference the most current version of the JavaScript API for Office library from the content delivery network (CDN). To do this, add the following  `script` tag to your HTML. Using `/1/` in the CDN URL ensures that you reference the most recent version of Office.js.

```HTML
<script src="https://appsforoffice.microsoft.com/lib/1/hosted/Office.js" type="text/javascript"></script>
```

## Options to specify Office hosts or API requirements

When you specify Office hosts or API requirements, there are several factors to consider. The following diagram shows how to decide which technique to use in your add-in.

![Choose the best option for your add-in when specifying Office hosts or API requirements](../images/options-for-office-hosts.png)

- If your add-in runs in one Office host, set the **Hosts** element in the manifest. For more information, see [Set the Hosts element](#set-the-hosts-element).

- To set the minimum requirement set or API members that an Office host must support to run your add-in, set the  **Requirements** element in the manifest. For more information, see [Set the Requirements element in the manifest](#set-the-requirements-element-in-the-manifest).

- If you would like to provide additional functionality if specific requirement sets or API members are available in the Office host, perform a runtime check in your add-in's JavaScript code. For example, if your add-in runs in Excel 2016, use API members from the Excel JavaScript API to provide additional functionality. For more information, see [Use runtime checks in your JavaScript code](#use-runtime-checks-in-your-javascript-code).

## Set the Hosts element

To make your add-in run in one Office host application, use the  **Hosts** and **Host** elements in the manifest. If you don't specify the **Hosts** element, your add-in will run in all hosts.

For example, the following  **Hosts** and **Host** declaration specifies that the add-in will work with any release of Excel, which includes Excel on the web, Windows, and iPad.

```xml
<Hosts>
  <Host Name="Workbook" />
</Hosts>
```

The  **Hosts** element can contain one or more **Host** elements. The **Host** element specifies the Office host your add-in requires. The **Name** attribute is required and can be set to one of the following values.

| Name          | Office host applications                                                                  |
|:--------------|:------------------------------------------------------------------------------------------|
| Database      | Access web apps                                                                           |
| Document      | Word on Windows, Word on Mac, Word on iPad, Word on the web                               |
| Mailbox       | Outlook on Windows, Outlook on Mac, Outlook on the web, Outlook on Android, Outlook on iOS|
| Presentation  | PowerPoint on Windows, PowerPoint on Mac, PowerPoint on iPad, PowerPoint on the web       |
| Project       | Project on Windows                                                                        |
| Workbook      | Excel on Windows, Excel on Mac, Excel on iPad, Excel on the web                           |

> [!NOTE]
> The  `Name` attribute specifies the Office host application that can run your add-in. Office hosts are supported on different platforms and run on desktops, web browsers, tablets, and mobile devices. You can't specify which platform can be used to run your add-in. For example, if you specify `Mailbox`, both Outlook on Windows and on the web can be used to run your add-in.

> [!IMPORTANT]
> We no longer recommend that you create and use Access web apps and databases in SharePoint. As an alternative, we recommend that you use [Microsoft PowerApps](https://powerapps.microsoft.com/) to build no-code business solutions for web and mobile devices.


## Set the Requirements element in the manifest

The  **Requirements** element specifies the minimum requirement sets or API members that must be supported by the Office host to run your add-in. The **Requirements** element can specify both requirement sets and individual methods used in your add-in. In version 1.1 of the add-in manifest schema, the **Requirements** element is optional for all add-ins, except for Outlook add-ins.

> [!WARNING]
> Only use the **Requirements** element to specify critical requirement sets or API members that your add-in must use. If the Office host or platform doesn't support the requirement sets or API members specified in the **Requirements** element, the add-in won't run in that host or platform, and won't display in **My Add-ins**. Instead, we recommend that you make your add-in available on all platforms of an Office host, such as Excel on the web, Windows, and iPad. To make your add-in available on  _all_ Office hosts and platforms, use runtime checks instead of the **Requirements** element.

The following code example shows an add-in that loads in all Office host applications that support the following:

-  **TableBindings** requirement set, which has a minimum version of "1.1".

-  **OOXML** requirement set, which has a minimum version of "1.1".

-  **Document.getSelectedDataAsync** method.

```XML
<Requirements>
   <Sets DefaultMinVersion="1.1">
      <Set Name="TableBindings" MinVersion="1.1"/>
      <Set Name="OOXML" MinVersion="1.1"/>
   </Sets>
   <Methods>
      <Method Name="Document.getSelectedDataAsync"/>
   </Methods>
</Requirements>
```

- The  **Requirements** element contains the **Sets** and **Methods** child elements.

- The  **Sets** element can contain one or more **Set** elements. **DefaultMinVersion** specifies the default **MinVersion** value of all child **Set** elements.

- The  **Set** element specifies requirement sets that the Office host must support to run the add-in. The **Name** attribute specifies the name of the requirement set. The **MinVersion** specifies the minimum version of the requirement set. **MinVersion** overrides the value of **DefaultMinVersion**. For more information about requirement sets and requirement set versions that your API members belong to, see [Office Add-in requirement sets](/office/dev/add-ins/reference/requirement-sets/office-add-in-requirement-sets).

- The  **Methods** element can contain one or more **Method** elements. You can't use the **Methods** element with Outlook add-ins.

- The  **Method** element specifies an individual method that must be supported in the Office host where your add-in runs. The **Name** attribute is required and specifies the name of the method qualified with its parent object.

## Use runtime checks in your JavaScript code

You might want to provide additional functionality in your add-in if certain requirement sets are supported by the Office host. For example, you might want to use the Word JavaScript APIs in your existing add-in if your add-in runs in Word 2016. To do this, you use the [isSetSupported](/javascript/api/office/office.requirementsetsupport#issetsupported-name--minversion-) method with the name of the requirement set. **isSetSupported** determines, at runtime, whether the Office host running the add-in supports the requirement set. If the requirement set is supported, **isSetSupported** returns **true** and runs the additional code that uses the API members from that requirement set. If the Office host doesn't support the requirement set, **isSetSupported** returns **false** and the additional code won't run. The following code shows the syntax to use with **isSetSupported**.

```js
if (Office.context.requirements.isSetSupported(RequirementSetName, MinimumVersion))
{
   // Code that uses API members from RequirementSetName.
}

```

- _RequirementSetName_ (required) is a string that represents the name of the requirement set (e.g., "**ExcelApi**", "**Mailbox**", etc.). For more information about available requirement sets, see [Office Add-in requirement sets](/office/dev/add-ins/reference/requirement-sets/office-add-in-requirement-sets).
- _MinimumVersion_ (optional) is a string that specifies the minimum requirement set version that the host must support in order for the code within the `if` statement to run (e.g., "**1.9**").

> [!WARNING]
> When calling the **isSetSupported** method, the value of the `MinimumVersion` parameter (if specified) should be a string. This is because the JavaScript parser cannot differentiate between numeric values such as 1.1 and 1.10, where as it can for string values such as "1.1" and "1.10".
> The `number` overload is deprecated.

Use **isSetSupported** with the **RequirementSetName** associated with the Office host as follows.

|Office host|RequirementSetName|
|---|---|
|Excel|ExcelApi|
|OneNote|OneNoteApi|
|Outlook|Mailbox|
|Word|WordApi|

The **isSetSupported** method and the requirement sets for these hosts are available in the latest Office.js file on the CDN. If you don't use Office.js from the CDN, your add-in might generate exceptions because **isSetSupported** will be undefined. For more information, see [Specify the latest JavaScript API for Office library](#specify-the-latest-javascript-api-for-office-library).

The following code example shows how an add-in can provide different functionality for different Office hosts that might support different requirement sets or API members.

```js
if (Office.context.requirements.isSetSupported('WordApi', '1.1'))
{
    // Run code that provides additional functionality using the Word JavaScript API when the add-in runs in Word 2016 or later.
}
else if (Office.context.requirements.isSetSupported('CustomXmlParts'))
{
    // Run code that uses API members from the CustomXmlParts requirement set.
}
else
{
    // Run additional code when the Office host is not Word 2016 or later and does not support the CustomXmlParts requirement set.
}

```

## Runtime checks using methods not in a requirement set

Some API members don't belong to requirement sets. This only applies to API members that are part of the [JavaScript API for Office](/office/dev/add-ins/reference/javascript-api-for-office) namespace (anything under `Office.` except [Outlook Mailbox APIs](/javascript/api/outlook)), but not API members that belong to the [Word JavaScript API](/office/dev/add-ins/reference/overview/word-add-ins-reference-overview) (anything in `Word.`), [Excel JavaScript API](/office/dev/add-ins/reference/overview/excel-add-ins-reference-overview) (anything in `Excel.`), or [OneNote JavaScript API](/office/dev/add-ins/reference/overview/onenote-add-ins-javascript-reference) (anything in `OneNote.`) namespaces. When your add-in depends on a method that is not part of a requirement set, you can use the runtime check to determine whether the method is supported by the Office host, as shown in the following code example. For a complete list of methods that don't belong to a requirement set, see [Office Add-in requirement sets](/office/dev/add-ins/reference/requirement-sets/office-add-in-requirement-sets#methods-that-arent-part-of-a-requirement-set).

> [!NOTE]
> We recommend that you limit the use of this type of runtime check in your add-in's code.

The following code example checks whether the host supports  **document.setSelectedDataAsync**.

```js
if (Office.context.document.setSelectedDataAsync)
{
    // Run code that uses document.setSelectedDataAsync.
}
```


## See also

- [Office Add-ins XML manifest](add-in-manifests.md)
- [Office Add-in requirement sets](/office/dev/add-ins/reference/requirement-sets/office-add-in-requirement-sets)
- [Word-Add-in-Get-Set-EditOpen-XML](https://github.com/OfficeDev/Word-Add-in-Get-Set-EditOpen-XML)
