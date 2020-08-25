---
title: count 运算符 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 count 运算符。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 04/16/2020
ms.date: 08/18/2020
ms.openlocfilehash: 8c9a724f688ae8098a1119d4034afb2f17796483
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515717"
---
# <a name="count-operator"></a>count 运算符

返回输入记录集中的记录数。

## <a name="syntax"></a>语法

`T | count`

## <a name="arguments"></a>参数

*T*：待计算记录数的表格数据。

## <a name="returns"></a>返回

此函数返回包含单个记录且列类型为 `long` 的表。 唯一单元格的值是 *T* 中的记录数。 

## <a name="example"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
StormEvents | count
```
