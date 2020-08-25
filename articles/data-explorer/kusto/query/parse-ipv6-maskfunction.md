---
title: parse_ipv4_mask() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 parse_ipv6_mask() 函数。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
origin.date: 02/24/2020
ms.date: 08/18/2020
ms.openlocfilehash: 26a0a785cb859e0a7c2135919c0a37d1e7994b2e
ms.sourcegitcommit: f4bd97855236f11020f968cfd5fbb0a4e84f9576
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516091"
---
# <a name="parse_ipv6_mask"></a>parse_ipv6_mask()
 
将 IPv6/IPv4 字符串和网络掩码转换为规范的 IPv6 字符串表示形式。

```kusto
parse_ipv6_mask("127.0.0.1", 24) == '0000:0000:0000:0000:0000:ffff:7f00:0000'
parse_ipv6_mask(":fe80::85d:e82c:9446:7994", 120) == 'fe80:0000:0000:0000:085d:e82c:9446:7900'
```

## <a name="syntax"></a>语法

`parse_ipv6_mask(`*`Expr`*`, `*`PrefixMask`*`)`

## <a name="arguments"></a>参数

* *`Expr`* ：表示将转换为规范 IPv6 表示形式的 IPv6/IPv4 网络地址的字符串表达式。 字符串可以包含使用 [IP 前缀表示法](#ip-prefix-notation)的网络掩码。
* *`PrefixMask`* ：从 0 到 128 的整数，表示所考虑的最高有效位数。

## <a name="ip-prefix-notation"></a>IP 前缀表示法

IP 地址可以通过斜杠 (`/`) 字符使用 `IP-prefix notation` 定义。
斜杠 (`/`) 左边的 IP 地址是基本 IP 地址。 斜杠 (`/`) 右边的数字（1 到 127）是网络掩码中连续 1 位的数目。

## <a name="returns"></a>返回

如果转换成功，则结果将是表示规范 IPv6 网络地址的字符串。
如果转换未成功，结果将为 `null`。

## <a name="example"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
datatable(ip_string:string, netmask:long)
[
 // IPv4 addresses
 '192.168.255.255',     120,  // 120-bit netmask is used
 '192.168.255.255/24',  124,  // 120-bit netmask is used, as IPv4 address doesn't use upper 8 bits
 '255.255.255.255', 128,  // 128-bit netmask is used
 // IPv6 addresses
 'fe80::85d:e82c:9446:7994', 128,     // 128-bit netmask is used
 'fe80::85d:e82c:9446:7994/120', 124, // 120-bit netmask is used
 // IPv6 with IPv4 notation
 '::192.168.255.255',    128,  // 128-bit netmask is used
 '::192.168.255.255/24', 128,  // 120-bit netmask is used, as IPv4 address doesn't use upper 8 bits
]
| extend ip6_canonical = parse_ipv6_mask(ip_string, netmask)
```

|ip_string|网络掩码|ip6_canonical|
|---|---|---|
|192.168.255.255|120|0000:0000:0000:0000:0000:ffff:c0a8:ff00|
|192.168.255.255/24|124|0000:0000:0000:0000:0000:ffff:c0a8:ff00|
|255.255.255.255|128|0000:0000:0000:0000:0000:ffff:ffff:ffff|
|fe80::85d:e82c:9446:7994|128|fe80:0000:0000:0000:085d:e82c:9446:7994|
|fe80::85d:e82c:9446:7994/120|124|fe80:0000:0000:0000:085d:e82c:9446:7900|
|::192.168.255.255|128|0000:0000:0000:0000:0000:ffff:c0a8:ffff|
|::192.168.255.255/24|128|0000:0000:0000:0000:0000:ffff:c0a8:ff00|

