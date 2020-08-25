---
title: binary_or() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 binary_or()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: 5b877d5fce9553ac930ae80ed7b44561dc06ed78
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515634"
---
# <a name="binary_or"></a>binary_or()

返回两个值的位 `or` 运算的结果。 

```kusto
binary_or(x,y)
```

## <a name="syntax"></a>语法

`binary_or(`*num1*`,` *num2* `)`

## <a name="arguments"></a>参数

* num1、num2：长整型数字 。

## <a name="returns"></a>返回

返回对一对数字（num1 | num2）的逻辑 OR 运算。