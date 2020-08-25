---
title: binary_shift_right() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 binary_shift_right()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: 9c590421c06761351cee168f9e0e3aa1d8996ab2
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515638"
---
# <a name="binary_shift_right"></a>binary_shift_right()

返回针对一对数字的二进制右移运算。

```kusto
binary_shift_right(x,y) 
```

## <a name="syntax"></a>语法

`binary_shift_right(`num1`,` num2 `)` 

## <a name="arguments"></a>参数

* num1、num2：长整型数字 。

## <a name="returns"></a>返回

返回针对一对数字的二进制右移运算：num1 >> (num2%64)。
如果 n 为负，则返回 NULL 值。