---
title: monthofyear() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 monthofyear()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: 76737aae7c5baf1c236a828540fadce59b44b140
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841618"
---
# <a name="monthofyear"></a>monthofyear()

返回一个整数，该整数表示给定年份的第几个月。

另一个别名：getmonth()

```kusto
monthofyear(datetime("2015-12-14"))
```

**语法**

`monthofyear(`*a_date*`)`

**参数**

* `a_date`：`datetime`。

**返回**

给定年份的 `month number`。