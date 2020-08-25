---
title: ceiling() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 ceiling()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: acf134ab169b61bccf4a16085955575c6dba5744
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515747"
---
# <a name="ceiling"></a>ceiling()

计算大于或等于指定数值表达式的最小整数。

## <a name="syntax"></a>语法

`ceiling(`*x*`)`

## <a name="arguments"></a>参数

* x：一个实数。

## <a name="returns"></a>返回

* 大于或等于指定数值表达式的最小整数。 

## <a name="examples"></a>示例

```kusto
print c1 = ceiling(-1.1), c2 = ceiling(0), c3 = ceiling(0.9)
```

|c1|c2|c3|
|---|---|---|
|-1|0|1|