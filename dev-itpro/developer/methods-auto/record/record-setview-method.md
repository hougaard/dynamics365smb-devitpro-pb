---
title: "SetView Method"
ms.author: solsen
ms.custom: na
ms.date: 06/25/2019
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.service: "dynamics365-business-central"
author: solsen
---
[//]: # (START>DO_NOT_EDIT)
[//]: # (IMPORTANT:Do not edit any of the content between here and the END>DO_NOT_EDIT.)
[//]: # (Any modifications should be made in the .xml files in the ModernDev repo.)
# SetView Method
Sets the current sort order, key, and filters on a table.


## Syntax
```
 Record.SetView(String: String)
```
## Parameters
*Record*  
&emsp;Type: [Record](record-data-type.md)  
An instance of the [Record](record-data-type.md) data type.  

*String*  
&emsp;Type: [String](../string/string-data-type.md)  
A string that contains the sort order, key, and filter to set. The string format is the same as the SourceTableView Property on pages.
          



[//]: # (IMPORTANT: END>DO_NOT_EDIT)
## See Also
[Record Data Type](record-data-type.md)  
[Getting Started with AL](../../devenv-get-started.md)  
[Developing Extensions](../../devenv-dev-overview.md)