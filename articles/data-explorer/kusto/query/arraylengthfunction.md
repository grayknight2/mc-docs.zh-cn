---
title: array_length() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 array_length()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: cacec1860562f64b102239e8c1402ea008f8572f
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516101"
---
# <a name="array_length"></a>array_length()

计算动态数组中的元素数。

## <a name="syntax"></a>语法

`array_length(`array`)`

## <a name="arguments"></a>参数

* array：一个 `dynamic` 值。

## <a name="returns"></a>返回

*array* 中的元素数；如果 *array* 不是数组，则返回`null`。

## <a name="examples"></a>示例

```kusto
print array_length(parse_json('[1, 2, 3, "four"]')) == 4

print array_length(parse_json('[8]')) == 1

print array_length(parse_json('[{}]')) == 1

print array_length(parse_json('[]')) == 0

print array_length(parse_json('{}')) == null

print array_length(parse_json('21')) == null
```