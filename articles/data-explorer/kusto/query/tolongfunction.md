---
title: tolong() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 tolong()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: 4cfe9f6fd8fd6534c25b9bc938e4256816a0387f
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516107"
---
# <a name="tolong"></a>tolong()

将输入转换为 long（带符号的 64 位）数字表示形式。

```kusto
tolong("123") == 123
```

> [!NOTE]
> 尽可能首选使用 [long()](./scalar-data-types/long.md)。

## <a name="syntax"></a>语法

`tolong(`Expr`)`

## <a name="arguments"></a>参数

* Expr：将转换为 long 的表达式。 

## <a name="returns"></a>返回

如果转换成功，则结果将是一个 long 类型的数字。
如果转换未成功，结果将是 `null`。
 
