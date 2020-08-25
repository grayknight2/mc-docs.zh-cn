---
title: 删除列 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍数据资源管理器中的删除列。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/11/2020
ms.date: 08/18/2020
ms.openlocfilehash: 5a3d8674d4e927070df7e535531e904ef6c79457
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515887"
---
# <a name="drop-column"></a>.drop column

从表中删除列。
若要从一个表中删除多个列，请参阅[以下内容](#drop-table-columns)。

> [!WARNING]
> 此命令不可逆。 删除的列中的所有数据都将被删除。
> 将来用于添加该列的命令将无法恢复数据。

**语法**

`.drop` `column` *TableName* `.` *ColumnName*

## <a name="drop-table-columns"></a>删除表列

从一个表中删除多个列。

> [!WARNING]
> 此命令不可逆。 删除的列中的所有数据都将被删除。
> 将来用于添加这些列的命令将无法还原数据。

**语法**

`.drop` `table` *TableName* `columns` `(` *Col1* [`,` *Col2*]... `)`