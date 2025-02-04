---
title: "Getting Started with AL"
description: "Description of how to get started with the development environment"
author: SusanneWindfeldPedersen
ms.custom: na
ms.date: 04/01/2019
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.service: "dynamics365-business-central"
ms.assetid: a0ac492d-e3c8-4a76-87b4-b469e08c58e7
ms.author: solsen
caps.latest.revision: 18
---

 

# Getting Started with AL
To get started writing extensions for [!INCLUDE[d365fin_long_md](includes/d365fin_long_md.md)] you will need a [!INCLUDE[d365fin_long_md](includes/d365fin_long_md.md)] tenant, Visual Studio Code, and the [!INCLUDE[d365al_ext_md](../includes/d365al_ext_md.md)]. Visual Studio Code is a cross platform editor that you will use for coding and debugging.

## Steps to set up a sandbox environment and Visual Studio Code
Go through the following steps to set up a sandbox environment. With this you get sample code that compiles and runs with just a few commands. 

> [!NOTE]  
> If you want to create a container-based sandbox, see [Get started with the Container Sandbox Development Environment](devenv-get-started-container-sandbox.md). For information about which sandboxes you can choose, see [Choosing Your Dynamics 365 Business Central Development Sandbox Environment](devenv-sandbox-overview.md).

1) Sign up for a [Dynamics 365 Business Central sandbox](https://signup.microsoft.com/signup?sku=6a4a1628-9b9a-424d-bed5-4118f0ede3fd&ru=https%3A%2F%2Fbusinesscentral.dynamics.com%2FSandbox%2F%3FredirectedFromSignup%3D1). 
2) Download [Visual Studio Code](https://code.visualstudio.com/Download).  
3) Download the [[!INCLUDE[d365al_ext_md](../includes/d365al_ext_md.md)]](https://marketplace.visualstudio.com/items?itemName=ms-dynamics-smb.al). 
4) Press **Ctrl+,** to open the **user settings** window; here you can modify the [telemetry settings](devenv-get-started.md#telemetry-settings).
5) Press **Alt+A, Alt+L** to trigger the **AL Go!** command, and then choose **Microsoft cloud sandbox**.  
    > [!NOTE]  
    > If you want to change your configuration at a later point in time, choose **Add Configuration** on the right side, and then choose one of the available options.  
6) Enter the credentials you provided for the signup, and then symbols will automatically start downloading. To manually download the symbols, press **Ctrl+Shift+P** and select **AL: Download symbols**. 
7) Press **F5** to deploy and run the extension on your online sandbox tenant.  


You now have a HelloWorld sample that compiles and runs. The JSON files in the project are automatically updated with the settings that allows you to press **F5** to build and deploy the solution.

## Tips and tricks
+ Use **Ctrl+Space** to activate IntelliSense.
+ Always use the `.al` extension on new files.
+ Use the built-in [snippets for code](devenv-syntax.md#ExamplesOfSnippets) by starting typing `t` and pick from the list.
+ Create objects within the right object ranges, see [Object Ranges in Dynamics 365 Business Central](devenv-object-ranges.md).
+ Build and get inspired by our sample library on [GitHub](https://github.com/Microsoft/al).
+ Use **Ctrl+Shift+P** to clear the credentials cache if you want to deploy against a different environment.


## JSON file settings
There are two JSON files in the project; the `app.json` file and the `launch.json` file. The files are automatically generated for your project. For more information, see [JSON files](devenv-json-files.md).

## Telemetry settings
By default, Visual Studio Code is set up with a telemetry system to enable that data and errors are sent to Microsoft. If you do not want to send telemetry data, you can change the `telemetry.enableTelemetry` setting from `true` to `false`. 

To modify the telemetry setting, press **Ctrl+,** in Visual Studio Code and choose the **user settings** window, which opens the `settings.json` file, and then add `telemetry.enableTelemetry` and set it to `false`. 
```
"telemetry.enableTelemetry": false,
```

> [!TIP]  
> The `settings.json` file contains user and workspace settings, these options can be modified to suit your preference. If you want to modify Visual Studio Code editor options and functional behavior settings, see [User and Workspace Settings](https://code.visualstudio.com/docs/getstarted/settings).


## The symbol file
The symbol file contains metadata of the application. This is what your extension is being built on, and therefore the symbol file must be present. If it is not present, you will be prompted to download it. For more information about the platform symbol file, see [Symbols](devenv-symbols.md).

## Installing and publishing an extension
To make your extension available to users, the package must be published to a specific Microsoft Dynamics 365 Business Central Server instance. The extension can be installed for one or more tenants. For more information about how to install and publish an extension, see [How to: Publish and Install an Extension](devenv-how-publish-and-install-an-extension-v2.md). 

## Controlling user access to publishing extensions
The access to publishing extensions is controlled on a user or user group basis by the **D365 EXTENSION MGT** permission set. 

> [!NOTE]  
> If you add new permission sets and want to control the access to developing and publishing extensions, you must include indirect read and write permissions to the NavApp table (read – for downloading symbols, write – for publishing the app) in the permission set.

To prohibit a user from publishing, just remove the user from the **D365 EXTENSION MGT** permission set.

## Next steps
Now that you have the tools and the HelloWorld example up and running, you might want to try to create a small sample app in AL. This walkthrough guides you through how to create a simple app adding objects, code, and publishing the app to your tenant. For more information, see [Building Your First Sample Extension With Extension Objects, Install Code, and Upgrade Code](devenv-extension-example.md).

## See Also 
[AL Development Environment](devenv-reference-overview.md)  
[FAQ for Developing in AL](devenv-dev-faq.md)  
[Syntax](devenv-syntax.md)  
[Building Your First Sample Extension With Extension Objects, Install Code, and Upgrade Code](devenv-extension-example.md)  