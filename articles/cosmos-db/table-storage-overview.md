---
title: Azure 表存储概述
description: 使用 Azure 表存储（一种 NoSQL 数据存储）将结构化数据存储在云中。
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: overview
origin.date: 05/20/2019
ms.date: 05/18/2020
author: rockboyfor
ms.author: v-yeche
ms.reviewer: sngun
ms.openlocfilehash: 3e95db890543d0021e0ce8ed2bbdb54b43a0ce42
ms.sourcegitcommit: 436a5dd9a446fe7b6d3c4d5bc76b652f7848c1ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83393128"
---
# <a name="azure-table-storage-overview"></a>Azure 表存储概述

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Azure 表存储是一项用于在云中存储结构化 NoSQL 数据的服务，通过无架构设计提供键/属性存储。 因为表存储无架构，因此可以很容易地随着应用程序需求的发展使数据适应存储。 对于许多类型的应用程序来说，访问表存储数据速度快且经济高效，在数据量相似的情况下，其成本通常比传统 SQL 要低。

可以使用表存储来存储灵活的数据集，例如 Web 应用程序的用户数据、通讯簿、设备信息，或者服务需要的其他类型的元数据。 可以在表中存储任意数量的实体，并且一个存储帐户可以包含任意数量的表，直至达到存储帐户的容量极限。

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

## <a name="next-steps"></a>后续步骤

* [Microsoft Azure 存储资源管理器](../vs-azure-tools-storage-manage-with-storage-explorer.md)是 Microsoft 提供的免费独立应用，可用于在 Windows、macOS 和 Linux 上以可视方式处理 Azure 存储数据。
    
    <!-- Notice: Remove from Microsoft -->
    
* [通过 .NET SDK 开始使用 Azure Cosmos DB 表 API 和 Azure 表存储](table-storage-how-to-use-dotnet.md)

* 查看表服务参考文档，了解有关可用 API 的完整详情：

    * [.NET 存储客户端库参考](https://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)

    * [REST API 参考](https://msdn.microsoft.com/library/azure/dd179355)

<!-- Update_Description: update meta properties, wording update, update link -->