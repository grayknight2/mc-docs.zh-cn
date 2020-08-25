---
title: avg()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 avg()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 09/26/2019
ms.date: 08/18/2020
ms.openlocfilehash: ebde8f4ee0b4bb6d70867551c26cf048d0944bf7
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515763"
---
# <a name="avg-aggregation-function"></a>avg()（聚合函数）

计算整组中 Expr 的平均值。 

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

## <a name="syntax"></a>语法

summarize `avg(`Expr`)`

## <a name="arguments"></a>参数

* Expr：用于聚合计算的表达式。 具有 `null` 值的记录会被忽略，并且不会包含在计算中。

## <a name="returns"></a>返回

整组中 Expr 的平均值。
 