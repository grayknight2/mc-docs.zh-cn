---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 07/08/2020
ms.date: 08/24/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: f77060f69115fa98ad8fa9b7ce7f57bccb356fe2
ms.sourcegitcommit: ecd6bf9cfec695c4e8d47befade8c462b1917cf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2020
ms.locfileid: "88753308"
---
下表描述了 Azure 常规用途 v1、v2、Blob 存储和块 blob 存储帐户的默认限制。 “流入量”限制是指针对发送到存储帐户的所有数据的限制。 “流出量”限制是指针对从存储帐户接收的所有数据的限制。

| 资源 | 限制 |
| --- | --- |
| 每个订阅每个区域的存储帐户数，包括标准存储帐户和高级存储帐户。| 250 |
| 最大存储帐户容量 | 5 PiB <sup>1</sup>|
| 每个存储帐户的 Blob 容器、Blob、文件共享、表、队列、实体或消息数目上限 | 无限制 |
| 每个存储帐户的最大请求速率<sup>1</sup> | 每秒 20,000 个请求 |
| 每个存储帐户的最大入口<sup>1</sup> | 如果已启用 RA-GRS/GRS，则为 5 Gbps；对于 LRS，为 10 Gbps<sup>2</sup> |
| 常规用途 v2 存储帐户和 Blob 存储帐户的最大出口（所有区域） | 50 Gbps |
| 常规用途 v1 存储帐户的最大出口 | 如果已启用 RA-GRS/GRS，则为 10 Gbps；对于 LRS，为 15 Gbps<sup>2</sup> |
| 每个存储帐户的最大虚拟网络规则数 | 200 |
| 每个存储帐户的最大 IP 地址规则数 | 200 |

<sup>1</sup> Azure 存储标准帐户根据请求支持更高的容量上限和更高的流入量上限。 若要请求增加帐户限制，请与 [Azure 支持](https://support.azure.cn/zh-cn/support/contact)联系。

<sup>2</sup> 如果存储帐户启用了异地冗余存储 (RA-GRS) 的读取访问权限，则辅助位置的出口目标与主位置的出口目标相同。 有关详细信息，请参阅 [Azure 存储复制](../articles/storage/common/storage-redundancy.md)。

> [!NOTE]
> 大多数情况下，Azure 建议使用常规用途 v2 存储帐户。 可以轻松将常规用途 v1 或 Azure Blob 存储帐户升级到常规用途 v2 帐户，无需停机且无需复制数据。 有关详细信息，请参阅[升级到常规用途 v2 存储帐户](../articles/storage/common/storage-account-upgrade.md)。

如果应用程序的需求超过单个存储帐户的伸缩性目标，则可以构建使用多个存储帐户的应用程序。 然后，可以将数据对象分布到这些存储帐户中。 有关批量定价的信息，请参阅 [Azure 存储定价](https://azure.cn/pricing/details/storage/)。

所有存储帐户都在平面网络拓扑上运行，而无论它们是何时创建的。 有关 Azure 存储的扁平网络体系结构和可伸缩性的详细信息，请参阅 [Azure 存储：具有高度一致性的高可用云存储服务](https://docs.microsoft.com/archive/blogs/hanuk/windows-azures-flat-network-storage-to-enable-higher-scalability-targets)。 
