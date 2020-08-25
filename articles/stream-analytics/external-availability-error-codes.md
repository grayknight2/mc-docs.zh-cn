---
title: 外部可用性错误代码 - Azure 流分析
description: 通过外部可用性错误代码排查 Azure 流分析问题。
ms.author: v-johya
author: Johnnytechn
ms.topic: troubleshooting
ms.date: 08/20/2020
ms.service: stream-analytics
ms.openlocfilehash: af538c2a68649d010bf502d9eec3f1a5c5aba3f0
ms.sourcegitcommit: 09c7071f4d0d9256b40a6bf700b38c6a25db1b26
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2020
ms.locfileid: "88715721"
---
# <a name="azure-stream-analytics-external-availability-error-codes"></a>Azure 流分析外部可用性错误代码

可以使用活动日志和资源日志来帮助调试 Azure 流分析作业中的意外行为。 本文列出了每个外部可用性错误代码的说明。 依赖服务不可用时，会发生外部可用性错误。

## <a name="externalserviceunavailable"></a>ExternalServiceUnavailable

* **原因**：服务暂时不可用。
* **建议**：流分析将继续尝试访问该服务。

## <a name="eventhubmessagingerror"></a>EventHubMessagingError

* **原因**：流分析在与 EventHub 通信时遇到错误。 


## <a name="next-steps"></a>后续步骤

* [对输入连接进行故障排除](stream-analytics-troubleshoot-input.md)
* [对 Azure 流分析输出进行故障排除](stream-analytics-troubleshoot-output.md)
* [对 Azure 流分析查询进行故障排除](stream-analytics-troubleshoot-query.md)
* [Azure 流分析数据错误](data-errors.md)

