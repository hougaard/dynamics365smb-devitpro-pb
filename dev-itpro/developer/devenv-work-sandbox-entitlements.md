---
title: "Working with Sandboxes and Entitlements"
description:
ms.author: freddyk
ms.reviewer: solsen
ms.custom: na
ms.date: 07/03/2019
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.service: "dynamics365-business-central"
author: freddydk
---

# Working with Sandboxes and Entitlements
The experience that a user has in [!INCLUDE[d365fin_long_md](includes/d365fin_long_md.md)] depends on the purchased subscription plan. In [!INCLUDE[d365fin_long_md](includes/d365fin_long_md.md)], there are two main plans; the Essential and the Premium plan, plus a few more. For more detailed information about the Essential and Premium plans, see [Business Central](https://dynamics.microsoft.com/en-us/business-central/overview/) on the Microsoft Dynamics 365 Marketing site. 

When you develop in a Docker sandbox, the Essential experience is automatically assigned to you (you set the experience on the **Company Information** page), which makes it difficult to test how a user with the Premium plan assigned will experience what you have developed.
<br>

## Setup for users with different plans
To mimic users with a specific subscription plan assigned, you can set them up with the user groups as detailed in the table below. 

> [!NOTE]  
> When you assign user groups the permissions in that user group are automatically assigned. Some user groups are not assigned by default, but the plan includes them.

> [!NOTE]  
> In the table below *non-default* means not assigned by default, but the plan allows this to be assigned to the user.

|User Name <br>The type of subscription plan <br> assigned to the given user|User Groups|Permission Sets|
|---------|-----------|---------------|
|EXTERNALACCOUNTANT|D365 EXT. ACCOUNTANT<br>D365 EXTENSION MGT<br>D365 TROUBLESHOOT (non-default)<br>D365 SECURITY (non-default)|D365 BUS FULL ACCESS<br>D365 EXTENSION MGT<br>D365 READ<br>LOCAL<br>TROUBLESHOOT TOOLS (non-default)<br>D365 BASIC (non-default)<br>SECURITY (non-default)|
|PREMIUM|D365 BUS PREMIUM<br>D365 EXTENSION MGT<br>D365 TROUBLESHOOT (non-default)<br>D365 SECURITY (non-default)|D365 BUS PREMIUM<br>D365 EXTENSION MGT<br>LOCAL<br>TROUBLESHOOT TOOLS (non-default)<br>D365 BASIC (non-default)<br>SECURITY (non-default)|
|ESSENTIAL|D365 BUS FULL ACCESS<br>D365 EXTENSION MGT<br>D365 TROUBLESHOOT (non-default)<br>D365 SECURITY (non-default)|D365 BUS FULL ACCESS<br>D365 EXTENSION MGT<br>LOCAL<br>TROUBLESHOOT TOOLS (non-default)<br>D365 BASIC (non-default)<br>SECURITY (non-default)|
|INTERNALADMIN|D365 INTERNAL ADMIN<br>D365 TROUBLESHOOT<br>D365 SECURITY (non-default)|D365 READ<br>LOCAL<br>SECURITY<br>TROUBLESHOOT TOOLS<br>D365 BASIC (non-default)<br>SECURITY (non-default)|
|TEAMMEMBER|D365 TEAM MEMBER<br>D365 TROUBLESHOOT (non-default)<br>D365 SECURITY (non-default)|D365 READ<br>D365 TEAM MEMBER<br>LOCAL<br>TROUBLESHOOT TOOLS (non-default)<br>D365 BASIC (non-default)<br>SECURITY (non-default)|
|DELEGATEDADMIN|D365 EXTENSION MGT<br>D365 FULL ACCESS<br>D365 RAPIDSTART<br>D365 TROUBLESHOOT<br>D365 SECURITY (non-default)|D365 BASIC<br>D365 EXTENSION MGT<br>D365 FULL ACCESS<br>D365 RAPIDSTART<br>LOCAL<br>TROUBLESHOOT TOOLS<br>D365 BASIC (non-default)<br>SECURITY (non-default)|

> [!TIP]  
> For more information about how to choose a user experience, see [Changing Which Features are Displayed](https://docs.microsoft.com/en-us/dynamics365/business-central/ui-experiences#choosing-a-user-experience-to-show-or-hide-features).

## Assigning the Premium plan to test users
Depending on how you are running your Docker sandbox, you assign the experience in different ways.

### Azure VMs
If you use [http://aka.ms/bcsandbox](http://aka.ms/bcsandbox) to create your [!INCLUDE[d365fin_long_md](includes/d365fin_long_md.md)] Sandbox Container Azure VM, the Azure Resource Manager template has two fields; **Assign Premium Plan** and **Create Test Users**, which by default are set to **Yes**.

**Assign Premium Plan** specifies whether or not your admin user should be assigned a Premium plan. **Create Test Users** specifies whether or not you want the setup to include test users. 

### NavContainerHelper
If you are using `New-NavContainer` to create your [!INCLUDE[d365fin_long_md](includes/d365fin_long_md.md)] Sandbox container, you must make sure that you are using version 0.2.8.3 or later.

Use the switch `assignPremiumPlan` on `New-NavContainer` like this:

```
New-NavContainer -accept_eula -assignPremiumPlan -containerName test -imageName microsoft/bcsandbox
```

This assigns the Premium plan to your default admin user. Internally this just adds a record to the **User Plan** table.

To create the test users, you must call the `Setup-NavContainerTestUsers` method:

```
Setup-NavContainerTestUsers containerName test -tenant default -password $securePassword
```

specifying the container and the password that you want to use for the new users.

Internally, the `Setup-NavContainerTestUsers` downloads an app which exposes an API, publishes and installs the app, and then invokes the `CreateTestUsers` API with the password needed. After this, the app is uninstalled and unpublished.

If you want to see code behind the app, it is available [here](https://dev.azure.com/businesscentralapps/CreateTestUsers).

### Docker run
If you are using Docker run to run your containers, you have a little more work to do.

First of all, you must override the `SetupNavUsers.ps1` by sharing a local folder to `c:\run\my` in the container and place a file called `SetupNavUsers.ps1` in that folder with the following content:

```
# Invoke default behavior
. (Join-Path $runPath $MyInvocation.MyCommand.Name)
 
Get-NavServerUser -serverInstance NAV -tenant default |? LicenseType -eq "FullUser" | % {
    $UserId = $_.UserSecurityId
    Write-Host "Assign Premium plan for $($_.Username)"
    sqlcmd -S 'localhostSQLEXPRESS' -d $DatabaseName -Q "INSERT INTO [dbo].[User Plan] ([Plan ID],[User Security ID]) VALUES ('{8e9002c0-a1d8-4465-b952-817d2948e6e2}','$userId')" | Out-Null
}
```

This will assign the Premium plan to the admin user in the database.

> [!TIP]  
> To set up test users, you can clone the [createtestusers](https://github.com/NAVDEMO/CreateTestUsers) repository and modify the code to create the users on the `oninstall` trigger with the password that you want.

## See Also
[Programming in AL](devenv-programming-in-al.md)  
[Choosing Your Dynamics 365 Business Central Development Sandbox Environment](devenv-sandbox-overview.md)  
[Container Sandbox](devenv-get-started-container-sandbox.md)  
[Changing Which Features are Displayed](https://docs.microsoft.com/en-us/dynamics365/business-central/ui-experiences#choosing-a-user-experience-to-show-or-hide-features)
