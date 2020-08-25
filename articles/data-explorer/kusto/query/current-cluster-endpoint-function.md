---
title: current_cluster_endpoint() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了 Azure 数据资源管理器中的 current_cluster_endpoint()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/18/2020
ms.openlocfilehash: 77365c7f6a5c39d8fa27340ce579ee67acb148c1
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515719"
---
# <a name="current_cluster_endpoint"></a>current_cluster_endpoint()

返回所查询当前群集的网络终结点（DNS 名称）。

## <a name="syntax"></a>语法

`current_cluster_endpoint()`

## <a name="returns"></a>返回

所查询当前群集的网络终结点（DNS 名称），`string` 类型的值。

## <a name="example"></a>示例

```kusto
print strcat("This query executed on: ", current_cluster_endpoint())
```