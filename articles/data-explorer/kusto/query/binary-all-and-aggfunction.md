---
title: binary_all_or()（聚合函数）- Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 binary_all_and()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/24/2020
ms.date: 08/18/2020
ms.openlocfilehash: e6b7a6b21bd08975f905b91923e9d2b0fecf19f9
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515639"
---
# <a name="binary_all_and-aggregation-function"></a>binary_all_or()（聚合函数）

使用每个汇总组的二进制 `AND` 操作累计值（如果没有分组就完成了汇总，则返回总计值）。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

## <a name="syntax"></a>语法

summarize `binary_all_and(`*Expr*`)`

## <a name="arguments"></a>参数

* *Expr*：长整型数字。

## <a name="returns"></a>返回

返回使用二进制 `AND` 操作对每个汇总组的记录进行聚合所得的值（如果没有分组就完成了汇总，则返回总计值）。

## <a name="example"></a>示例

使用二进制 `AND` 操作生成“cafe-food”：

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(num:long)
[
  0xFFFFFFFF, 
  0xFFFFF00F,
  0xCFFFFFFD,
  0xFAFEFFFF,
]
| summarize result = toupper(tohex(binary_all_and(num)))
```

|result|
|---|
|CAFEF00D|
