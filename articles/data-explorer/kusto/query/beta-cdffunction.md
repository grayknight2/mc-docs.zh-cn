---
title: beta_cdf() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 beta_cdf()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: 11c4a2a09498dac5fb34a56747d3497726f7eb2c
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516040"
---
# <a name="beta_cdf"></a>beta_cdf()

返回标准的累积 beta 分布函数。

```kusto
beta_cdf(0.2, 10.0, 50.0)
```

若 *probability* = `beta_cdf(`*x*,...`)`，则 `beta_inv(`*probability*,...`)` = *x*。

Beta 分布通常用于研究样本中某个因素的变化情况（用百分数表示），如一天中人们看电视所花时间的比例。

## <a name="syntax"></a>语法

`beta_cdf(`*x*`, `*alpha*`, `*beta*`)`

## <a name="arguments"></a>参数

* x：用于计算函数的值。
* alpha：分布的一个参数。
* beta：分布的一个参数。

## <a name="returns"></a>返回

* 累积 beta 分布函数。

**说明**

如果任何参数不是数字，则 beta_cdf() 将返回 null 值。

如果 x < 0 或 x > 1，则 beta_cdf() 返回 NAN 值。

如果 alpha ≤ 0 或 beta ≤ 0，则 beta_cdf() 将返回 NAN 值。

## <a name="examples"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(x:double, alpha:double, beta:double, comment:string)
[
    0.9, 10.0, 20.0, "Valid input",
    1.5, 10.0, 20.0, "x > 1, yields NaN",
    double(-10), 10.0, 20.0, "x < 0, yields NaN",
    0.1, double(-1.0), 20.0, "alpha is < 0, yields NaN"
]
| extend b = beta_cdf(x, alpha, beta)
```

|x|alpha|beta|comment|b|
|---|---|---|---|---|
|0.9|10 个|20 个|有效输入|0.999999999999959|
|1.5|10 个|20 个|x > 1，产生 NAN|NaN|
|-10|10 个|20 个|x < 0，产生 NAN|NaN|
|0.1|-1|20 个|alpha < 0，产生 NAN|NaN|


**另请参阅**


* 若要计算 beta 累积概率密度函数的反函数，请参阅 [beta-inv()](./beta-invfunction.md)。
* 有关计算概率密度函数，请参阅 [beta-pdf()](./beta-pdffunction.md)。