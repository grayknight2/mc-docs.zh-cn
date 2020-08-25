---
title: Kusto IngestionTime 策略管理 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 IngestionTime 策略。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: 589e89ee0deaacfde9eb98b7d0a552f86ca333e5
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515910"
---
# <a name="ingestiontime-policy-command"></a>ingestiontime 策略命令

IngestionTime 策略是针对表设置的可选策略（默认启用）。
它提供将记录引入表的大致时间。

可以使用 `ingestion_time()` 函数在查询时访问引入时间值。

```kusto
T | extend ingestionTime = ingestion_time()
```

启用/禁用策略：
```kusto
.alter table table_name policy ingestiontime true
```

启用/禁用多个表的策略：
```kusto
.alter tables (table_name [, ...]) policy ingestiontime true
```

查看策略：
```kusto
.show table table_name policy ingestiontime  

.show table * policy ingestiontime  
```

删除策略（与禁用行为等效）：
```kusto
.delete table table_name policy ingestiontime  
```
