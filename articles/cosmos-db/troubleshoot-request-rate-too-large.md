---
title: 排除 Azure Cosmos DB 请求率太大的异常
description: 如何诊断和修复请求率太大的异常
author: rockboyfor
ms.service: cosmos-db
origin.date: 07/13/2020
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 5fec4dee2bff7b93f304ed35afb829fd967bdaca
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223480"
---
<!--Verified successfully-->
# <a name="diagnose-and-troubleshoot-cosmos-db-too-many-requests"></a>诊断并排除 Cosmos DB 请求太多的故障
“请求率太大”或错误代码 429 表明请求受到限制。

## <a name="troubleshooting-steps"></a>疑难解答步骤
下面的列表包含请求太多的已知原因和解决方案。

### <a name="1-check-the-metrics"></a>1.检查指标
客户应检查 [Azure Cosmos DB 监视](monitor-cosmos-db.md)，了解是否存在 429 异常。

## <a name="cause"></a>原因：
消耗的吞吐量 (RU/s) 超过[预配吞吐量](set-throughput.md)。 SDK 会根据指定的重试策略自动重试请求。 如果经常遇到这种失败，请考虑增大集合的吞吐量。 检查门户指标，以确定是否收到了 429 错误。 查看分区键，以确保[均匀分配存储和请求量](partition-data.md)。

## <a name="solution"></a>解决方案：
1. 使用[门户或 SDK](set-throughput.md) 增加预配的吞吐量。
2. 将数据库或容器切换为[自动缩放](provision-throughput-autoscale.md)。

## <a name="next-steps"></a>后续步骤
* [诊断和排查](troubleshoot-dot-net-sdk.md)使用 Azure Cosmos DB .NET SDK 时遇到的问题
* 了解 [.NET V3](performance-tips-dotnet-sdk-v3-sql.md) 和 [.NET V2](performance-tips.md) 的性能准则

<!-- Update_Description: new article about troubleshoot request rate too large -->
<!--NEW.date: 08/10/2020-->