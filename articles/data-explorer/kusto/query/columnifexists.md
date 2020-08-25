---
title: Column_ifexists() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 column_ifexists()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: 99b30387c004612a6dd2ced8d8407199d3f1996e
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516061"
---
# <a name="column_ifexists"></a>column_ifexists()

采用列名作为字符串和默认值。 返回对列的引用（如果存在），否则返回默认值。

## <a name="syntax"></a>语法

`column_ifexists(`*columnName*`, `*defaultValue*)

## <a name="arguments"></a>参数

* *columnName*：列的名称
* *defaultValue*：在使用函数的上下文中不存在列时要使用的值。
                  此值可以是任何标量表达式（例如，对另一列的引用）。

## <a name="returns"></a>返回

如果“columnName”存在，则使用它引用的列。 否则使用“defaultValue”。

## <a name="examples"></a>示例

```kusto
.create function with (docstring = "Wraps a table query that allows querying the table even if columnName doesn't exist ", folder="My Functions")
ColumnOrDefault(tableName:string, columnName:string)
{
    // There's no column "Capital" in "StormEvents", therefore, the State column will be used instead
    table(tableName) | project column_ifexists(columnName, State)
}


ColumnOrDefault("StormEvents", "Captial");
```