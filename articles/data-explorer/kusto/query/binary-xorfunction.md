---
title: Binary_xor() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 binary_xor()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: 4b6e0e685a0b6e597314c31c83d600d308233fca
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516062"
---
# <a name="binary_xor"></a>binary_xor()

返回两个值的位 `xor` 运算的结果。

```kusto
binary_xor(x,y)
```

## <a name="syntax"></a>语法

`binary_xor(`*num1*`,` *num2* `)`

## <a name="arguments"></a>参数

* num1、num2：长整型数字 。

## <a name="returns"></a>返回

返回对一对数字的逻辑 XOR 运算：num1 ^ num2。