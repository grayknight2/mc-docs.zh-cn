---
title: bin_auto() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 bin_auto()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: c137547afab28de019c8f63552109ad492950f79
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516030"
---
# <a name="bin_auto"></a>bin_auto()

将值向下舍入为固定大小的“bin”，并由查询属性提供对 bin 大小和起点的控制。

## <a name="syntax"></a>语法

`bin_auto` `(` *Expression* `)`

## <a name="arguments"></a>参数

* *表达式*：数值类型的标量表达式，指示要舍入的值。

**客户端请求属性**

* `query_bin_auto_size`：指示每个 bin 大小的数值文本。
* `query_bin_auto_at`：指示 Expression 的一个值的数值文本，是一个“固定点”（即，`bin_auto(fixed_point)` == `fixed_point` 的值 `fixed_point`）

## <a name="returns"></a>返回

Expression 下 `query_bin_auto_at` 最接近的倍数已移位，因此 `query_bin_auto_at` 将被转换为自身。

## <a name="examples"></a>示例

```kusto
set query_bin_auto_size=1h;
set query_bin_auto_at=datetime(2017-01-01 00:05);
range Timestamp from datetime(2017-01-01 00:05) to datetime(2017-01-01 02:00) step 1m
| summarize count() by bin_auto(Timestamp)
```

|Timestamp                    | count_|
|-----------------------------|-------|
|2017-01-01 00:05:00.0000000  | 60    |
|2017-01-01 01:05:00.0000000  | 56    |