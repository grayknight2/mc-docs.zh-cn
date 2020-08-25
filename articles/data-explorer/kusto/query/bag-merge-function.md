---
title: bag_merge() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 bag_merge()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: elgevork
ms.service: data-explorer
ms.topic: reference
origin.date: 06/18/2020
ms.date: 08/18/2020
ms.openlocfilehash: 7e28aea54da55adeaf1e1295f05c2647ea10c742
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515765"
---
# <a name="bag_merge"></a>bag_merge()

将 `dynamic` 属性包合并到合并了所有属性的 `dynamic` 属性包中。

## <a name="syntax"></a>语法

`bag_merge(`bag1`, `bag2`[`,` *bag3*, ...])` 

## <a name="arguments"></a>参数

* *bag1...bagN*：输入 `dynamic` 属性包。 该函数接受 2 到 64 个参数。

## <a name="returns"></a>返回

返回 `dynamic` 属性包。 来自合并所有输入属性包对象的结果。 如果一个键出现在多个输入对象中，则将（从此键的可能值中）选择一个任意值。

## <a name="example"></a>示例

表达式：

<!-- csl: https://help.kusto.chinacloudapi.cn:443/Samples -->
```kusto
print result = bag_merge(
   dynamic({'A1':12, 'B1':2, 'C1':3}),
   dynamic({'A2':81, 'B2':82, 'A1':1}))
```

|result|
|---|
|{<br>  "A1":12,<br>  "B1":2,<br>  "C1":3,<br>  "A2":81,<br>  "B2":82<br>}|
