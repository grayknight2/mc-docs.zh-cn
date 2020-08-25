---
title: todynamic()、toobject() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 todynamic()、toobject()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 08/18/2020
ms.openlocfilehash: 266d8b12c3dd08c54980334b97eb3f169fbad761
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516109"
---
# <a name="todynamic-toobject"></a>todynamic(), toobject()

将 `string` 解释为 [JSON 值](https://json.org/)并以 [`dynamic`](./scalar-data-types/dynamic.md) 形式返回值。 

当需要提取 JSON 复合对象的多个元素时，使用它比使用 [extractjson() 函数](./extractjsonfunction.md)更好。

[parse_json()](./parsejsonfunction.md) 函数的别名。

> [!NOTE]
> 尽可能首选使用 [dynamic()](./scalar-data-types/dynamic.md)。

## <a name="syntax"></a>语法

`todynamic(`*json*`)`
`toobject(`*json*`)`

## <a name="arguments"></a>参数

* json：JSON 文档。

## <a name="returns"></a>返回

*json* 指定的 `dynamic` 类型对象。
