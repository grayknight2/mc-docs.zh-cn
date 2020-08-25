---
title: binary_all_xor()（聚合函数）- Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 binary_all_xor()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/06/2020
ms.date: 08/18/2020
ms.openlocfilehash: 9ca380dbaca4aee8d7fcc748080f7b2ddee7d54c
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516028"
---
# <a name="binary_all_xor-aggregation-function"></a>binary_all_xor()（聚合函数）

使用每个汇总组的二进制 `XOR` 操作累计值（如果没有分组就完成了汇总，则返回总计值）。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

## <a name="syntax"></a>语法

summarize `binary_all_xor(`Expr`)`

## <a name="arguments"></a>参数

* *Expr*：长整型数字。

## <a name="returns"></a>返回

返回使用二进制 `XOR` 操作对每个汇总组的记录进行聚合所得的值（如果没有分组就完成了汇总，则返回总计值）。

## <a name="example"></a>示例

使用二进制 `XOR` 操作生成“cafe-food”：

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(num:long)
[
  0x44404440,
  0x1E1E1E1E,
  0x90ABBA09,
  0x000B105A,
]
| summarize result = toupper(tohex(binary_all_xor(num)))
```

|result|
|---|
|CAFEF00D|
