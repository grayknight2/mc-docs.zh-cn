---
title: alter-merge table column-docstrings - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 alter-merge table column-docstrings。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: 7cbd371d39dc62a88b48f6f79ad145597e1f45db
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515697"
---
# <a name="alter-merge-table-column-docstrings"></a>.alter-merge table column-docstrings

设置指定表的一个或多个列的 `docstring` 属性。 未显式设置的列保留此属性的现有值（如果有）。

有关 alter table column-docstring 的信息，请参阅 [下方内容](#alter-table-column-docstrings)。

**语法**

`.alter-merge` `table` *TableName* `column-docstring` `(` *Col1* `:` *Docstring1* [`,` *Col2* `:` *Docstring2*]... `)`

**示例** 

```kusto
.alter-merge table Table1 column-docstrings (Column1:"DocString1", Column2:"DocString2")
```

## <a name="alter-table-column-docstrings"></a>alter table column-docstrings

设置指定表的一个或多个列的 `docstring` 属性。 未显式设置的列将删除此属性。

**语法**

`.alter` `table` *TableName* `column-docstring` `(` *Col1* `:` *Docstring1* [`,` *Col2* `:` *Docstring2*]... `)`

**示例** 

```kusto
.alter table Table1 column-docstrings (Column1:"DocString1", Column2:"DocString2")
```
