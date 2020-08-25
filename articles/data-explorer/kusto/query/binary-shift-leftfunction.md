---
title: binary_shift_left() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 binary_shift_left()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: 84263bb6d5ec389f89b8f4320b8b95280e1b34f5
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515633"
---
# <a name="binary_shift_left"></a>binary_shift_left()

返回针对一对数字的二进制左移运算。

```kusto
binary_shift_left(x,y)  
```

## <a name="syntax"></a>语法

`binary_shift_left(`num1`,` num2 `)` 

## <a name="arguments"></a>参数

* num1、num2：int 数字 。

## <a name="returns"></a>返回

返回针对一对数字的二进制左移运算：num1 << (num2%64)。
如果 n 为负，则返回 NULL 值。