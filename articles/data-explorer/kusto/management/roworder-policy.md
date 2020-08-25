---
title: RowOrder 策略 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 RowOrder 策略。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/24/2020
ms.date: 08/18/2020
ms.openlocfilehash: 52a04c3b01c0a8c0f9e287f9fddb1e9a80ef95af
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515666"
---
# <a name="roworder-policy-command"></a>roworder 策略命令

本文介绍用于创建和更改[行顺序策略](../management/roworderpolicy.md)的控制命令。

## <a name="show-roworder-policy"></a>显示 RowOrder 策略

```kusto
.show table <table_name> policy roworder

.show table * policy roworder
```

## <a name="delete-roworder-policy"></a>删除 RowOrder 策略

```kusto
.delete table <table_name> policy roworder
```

## <a name="alter-roworder-policy"></a>更改 RowOrder 策略

```kusto
.alter table <table_name> policy roworder (<row_order_policy>)

.alter tables (<table_name> [, ...]) policy roworder (<row_order_policy>)

.alter-merge table <table_name> policy roworder (<row_order_policy>)
```

**示例** 

下面的示例将 `TenantId` 列上的行顺序策略（升序）设置为主键，将 `Timestamp` 列上的行顺序策略（升序）设置为辅助键。 然后查询策略。

```kusto
.alter table events policy roworder (TenantId asc, Timestamp desc)

.alter tables (events1, events2, events3) policy roworder (TenantId asc, Timestamp desc)

.show table events policy roworder 
```

|TableName|RowOrderPolicy| 
|---|---|
|events|(TenantId asc, Timestamp desc)|
