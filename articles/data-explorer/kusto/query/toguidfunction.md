---
title: toguid() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 toguid()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: 301261c3b7743b8bc035eabaffb4fe9d52d335d6
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516108"
---
# <a name="toguid"></a>toguid()

将输入转换为 [`guid`](./scalar-data-types/guid.md) 表现形式。

```kusto
toguid("70fc66f7-8279-44fc-9092-d364d70fce44") == guid("70fc66f7-8279-44fc-9092-d364d70fce44")
```

> [!NOTE]
> 如果可能，首选使用 [guid()](./scalar-data-types/guid.md)。

## <a name="syntax"></a>语法

`toguid(`Expr`)`

## <a name="arguments"></a>参数

* Expr：将转换为 [`guid`](./scalar-data-types/guid.md) 标量的表达式。 

## <a name="returns"></a>返回

如果转换成功，结果将是 [`guid`](./scalar-data-types/guid.md) 标量。
如果转换未成功，结果将是 `null`。
