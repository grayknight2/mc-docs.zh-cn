---
title: dcountif()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 dcountif()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 3690305b4347c18e27a7db2ba2919163acf83c47
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841586"
---
# <a name="dcountif-aggregation-function"></a>dcountif()（聚合函数）

返回对行的 *Expr* 非重复值数的估计值，其 *Predicate* 的求值为 `true`。 

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用。

了解[估算精确度](dcount-aggfunction.md#estimation-accuracy)。

**语法**

summarize `dcountif(`*Expr*, *Predicate*, [`,` *Accuracy*]`)`

**参数**

* Expr：用于聚合计算的表达式。
* *谓词*：将用于筛选行的表达式。
* 如果指定，*Accuracy* 可控制速度和准确性之间的平衡。
    * `0` = 最低准确性和最快计算。 错误率为 1.6%
    * `1` = 默认值，可平衡准确度和计算时间；错误率约 0.8%。
    * `2` = 精确的慢速计算；错误率约 0.4%。
    * `3` = 更精确的慢速计算；错误率约 0.28%。
    * `4` = 超级精确的最慢计算；错误率约 0.2%。
    
**返回**

返回对组中行的 *Expr* 非重复值数的估计值，这些行的 *Predicate* 的求值为 `true`。 

**示例**

```kusto
PageViewLog | summarize countries=dcountif(country, country startswith "United") by continent
```

**提示：偏移量错误**

在 `Predicate` 表达式的所有行都通过或没有行通过的极端情况下，`dcountif()` 可能导致一次性错误