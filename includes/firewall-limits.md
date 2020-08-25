---
title: include 文件
description: include 文件
services: firewall
author: rockboyfor
ms.service: firewall
ms.topic: include
origin.date: 07/30/2020
ms.date: 08/10/2020
ms.testscope: no
ms.testdate: 06/15/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 922202b2c7fa2c44086f4f7e66e083f986313f3d
ms.sourcegitcommit: ac70b12de243a9949bf86b81b2576e595e55b2a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "88753615"
---
| 资源 | 限制 |
| --- | --- |
| 数据吞吐量 |30 Gbps<sup>1</sup> |
|规则|10,000。 已组合所有规则类型。|
|最高 DNAT 规则数目|单个公共 IP 地址 298 个。<br />任何其他公共 IP 地址都有助于可用的 SNAT 端口，但会减少可用的 DNAT 规则数。 例如，两个公共 IP 地址允许 297 个 DNAT 规则。 如果同时为 TCP 和 UDP 配置规则的协议，则会将其计为两个规则。|
|最小 AzureFirewallSubnet 大小 |/26|
|网络和应用程序规则的端口范围|1 - 65535|
|公共 IP 地址|最多 250 个。 所有公共 IP 地址都可以在 DNAT 规则中使用，它们都对可用的 SNAT 端口有影响。|
|路由表|默认情况下，AzureFirewallSubnet 使用其 NextHopType 值设置为“Internet”的 0.0.0.0/0 路由。<br /><br />Azure 防火墙必须具有直接的 Internet 连接。 如果 AzureFirewallSubnet 知道通过 BGP 的本地网络的默认路由，则必须将其替代为 0.0.0.0/0 UDR，将 NextHopType 值设置为 Internet 以保持 Internet 直接连接 。 默认情况下，Azure 防火墙不支持强制的安全加密链路连接到本地网络。<br /><br />但是，如果你的配置要求通过强制隧道连接到本地网络，Azure 将基于具体的情况提供支持。 请联系支持人员，以便我们可以查看你的情况。 如果接受，我们将允许你的订阅，并确保保持所需的防火墙 Internet 连接。|

<!--MOONCAKE: IP Group not avilabl till on 08/07/2020-->

<sup>1</sup>如果需要增加这些限制，请与 Azure 支持部门联系。

<!-- Update_Description: update meta properties, wording update, update link -->