---
title: coalesce() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 coalesce()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: 417e2ebdee6f099aa8861a5b0c92efc212f16eba
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515816"
---
# <a name="coalesce"></a>coalesce()

计算表达式列表，并返回第一个非 null（或非空，适用于字符串）表达式。

```kusto
coalesce(tolong("not a number"), tolong("42"), 33) == 42
```

## <a name="syntax"></a>语法

`coalesce(`*expr_1*`, `*expr_2*`,` ...)

## <a name="arguments"></a>参数

* expr_i：要计算的标量表达式。
- 所有参数的类型必须相同。
- 最多支持 64 个参数。

## <a name="returns"></a>返回

其值不为 null（或不为空，适用于字符串表达式）的第一个 expr_i 的值。

## <a name="example"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples  -->
```kusto
print result=coalesce(tolong("not a number"), tolong("42"), 33)
```

|result|
|---|
|42|
