---
title: consume 运算符 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 consume 运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/30/2020
ms.date: 08/18/2020
ms.openlocfilehash: 7cc41d9094ed9df5d246c2ba68e57638f247aade
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516060"
---
# <a name="consume-operator"></a>consume 运算符

使用传递给运算符的表格格式数据流。 

`consume` 运算符主要用于触发查询副作用，实际上不会将结果返回给调用方。

```kusto
T | consume
```

## <a name="syntax"></a>语法

`consume` [`decodeblocks` `=` *DecodeBlocks*]

## <a name="arguments"></a>参数

* *DecodeBlocks*：一个常量布尔值。 如果将其设置为 `true`，或者将请求属性 `perftrace` 设置为 `true`，则 `consume` 操作符不仅会枚举其输入中的记录，实际上也会强制解压缩和解码这些记录中的每个值。

`consume` 运算符可用于估算查询的成本，实际上不会将结果传递回客户端。
（由于多种原因，估算不准确。例如，`consume` 是进行分布式计算的，因此 `T | consume` 不会在群集的节点之间传输表的数据。）

<!--
* *WithStats*: A constant Boolean value. If set to `true` (or if the global
  property `perftrace` is set), the operator will return a single
  row with a single column called `Stats` of type `dynamic` holding the statistics
  of the data source fed to the `consume` operator.
-->