---
title: 排除 Azure Cosmos DB 服务请求超时异常的故障
description: 如何诊断和修复 Cosmos DB 服务请求超时异常
author: rockboyfor
ms.service: cosmos-db
origin.date: 07/13/2020
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: d8fc0736264799b97d30b2e1f16b6b674009f3ab
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223592"
---
<!--Verified successfully-->
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-request-timeout"></a>诊断和排除 Azure Cosmos DB 请求超时故障
Azure Cosmos DB 返回了 HTTP 408 请求超时

## <a name="troubleshooting-steps"></a>疑难解答步骤
下面的列表包含请求超时异常的已知原因和解决方案。

### <a name="1-check-the-sla"></a>1.检查 SLA
客户应检查 [Azure Cosmos DB 监视](monitor-cosmos-db.md)，了解 408 异常是否违反 Cosmos DB SLA。

#### <a name="solution-1-it-did-not-violate-the-cosmos-db-sla"></a>解决方案 1：它不违反 Cosmos DB SLA
应用程序应处理此方案，并在发生这些暂时性故障时重试。

#### <a name="solution-2-it-did-violate-the-cosmos-db-sla"></a>解决方案 2：它确实违反了 Cosmos DB SLA
联系 Azure 支持： https://portal.azure.cn/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview

### <a name="2-hot-partition-key"></a>2.热分区键
Azure Cosmos DB 在物理分区之间均匀分配预配的总吞吐量。 存在热分区时，物理分区上的一个或多个逻辑分区键会消耗该物理分区的全部 RU/s，而其他物理分区上的 RU/s 则不会被使用。 作为一种症状，所消耗的 RU/s 总数将小于数据库或容器上的整体预配 RU/s，但针对该热逻辑分区键将会显示请求限制 (429s)。

<!--Not Available on [Normalized RU Consumption metric](monitor-normalized-request-units.md)-->

#### <a name="solution"></a>解决方案：
选择均匀分配请求量和存储的适当分区键。 了解如何[更改分区键](https://devblogs.microsoft.com/cosmosdb/how-to-change-your-partition-key/)。

## <a name="next-steps"></a>后续步骤
* [诊断和排查](troubleshoot-dot-net-sdk.md)使用 Azure Cosmos DB .NET SDK 时遇到的问题
* 了解 [.NET V3](performance-tips-dotnet-sdk-v3-sql.md) 和 [.NET V2](performance-tips.md) 的性能准则

<!-- Update_Description: new article about troubleshoot request timeout -->
<!--NEW.date: 08/10/2020-->