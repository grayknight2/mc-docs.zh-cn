---
title: asin() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 asin()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 08/18/2020
ms.openlocfilehash: de3c42bcedf9e340c8908ad44c16a3d03d8baeb0
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515783"
---
# <a name="asin"></a>asin()

返回其正弦为指定数字的角度（[`sin()`](sinfunction.md) 的反运算）。

## <a name="syntax"></a>语法

`asin(`*x*`)`

## <a name="arguments"></a>参数

* x：范围 [-1, 1] 中的实数。

## <a name="returns"></a>返回

* `x` 的反正弦的值
* `null` 如果 `x` < -1 或者 `x` > 1