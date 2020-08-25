---
title: Azure Cosmos DB 查询语言中的日期和时间函数
description: 了解 Azure Cosmos DB 中的日期和时间 SQL 系统函数，以便执行日期/时间和时间戳操作。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 07/09/2020
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 22d5062e5888e0628577db832116931d5cc8f35c
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88222632"
---
# <a name="date-and-time-functions-azure-cosmos-db"></a>日期和时间函数 (Azure Cosmos DB)

通过日期和时间函数，可以在 Azure Cosmos DB 中执行 DateTime 和时间戳操作。

## <a name="functions-to-obtain-the-date-and-time"></a>用于获取日期和时间的函数

使用以下标量函数可以获取采用以下两种格式的当前 UTC 日期和时间：一个符合 ISO 8601 格式的字符串，或一个其值为以毫秒为单位的 Unix 纪元的数值时间戳：

* [GetCurrentDateTime](sql-query-getcurrentdatetime.md)
* [GetCurrentTimestamp](sql-query-getcurrenttimestamp.md)

## <a name="functions-to-work-with-datetime-values"></a>用于处理 DateTime 值的函数

以下函数使你可以轻松地处理 DateTime 值：

* [DateTimeAdd](sql-query-datetimeadd.md)
* [DateTimeDiff](sql-query-datetimediff.md)
* [DateTimeFromParts](sql-query-datetimefromparts.md)

## <a name="next-steps"></a>后续步骤

- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)
- [用户定义的函数](sql-query-udfs.md)
- [聚合](sql-query-aggregates.md)

<!-- Update_Description: update meta properties, wording update, update link -->