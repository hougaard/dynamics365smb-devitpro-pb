---
title: "OnAction Trigger"
ms.custom: na
ms.date: 04/01/2019
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.service: "dynamics365-business-central"
ms.assetid: 05bae0df-26f3-4b16-9e21-1c79ab3d1737
author: SusanneWindfeldPedersen
manager: edupont
---



# OnAction Trigger
Runs when a user selects an action on a page.  

## Syntax  
```  
trigger OnAction()
begin
    ...
end;
```    

## Applies To  

-   Page actions  

## Remarks  
 This is called after the action properties, such as the [RunObject Property](../properties/devenv-runobject-property.md), are invoked.  

 You typically use the [RunObject Property](../properties/devenv-runobject-property.md) to run objects such as pages, reports, and codeunits from an action. You can use the OnAction trigger when you require more processing than the [RunObject Property](../properties/devenv-runobject-property.md) can support, such as filtering data before a page is displayed or writing to the database.  

> [!NOTE]  
>  The OnAction trigger is not used on Role Center pages. If you add AL code to the trigger, the Role Center page will compile, but the AL code will be ignored at runtime.  

## See Also  
 [RunObject Property](../properties/devenv-runobject-property.md)  
 [Page and Action Triggers](devenv-page-and-action-triggers.md)  
 [Page Properties](../properties/devenv-page-properties.md)  
 [Triggers](devenv-triggers.md)  
