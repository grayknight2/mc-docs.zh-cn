---
title: 使用 Azure 流分析错误代码进行故障排除
description: 通过内部错误代码排查 Azure 流分析问题。
ms.author: v-johya
author: Johnnytechn
ms.topic: troubleshooting
ms.date: 08/20/2020
ms.service: stream-analytics
ms.openlocfilehash: 67e8c545214eb2fb2042262585ef19aff2d0788c
ms.sourcegitcommit: 09c7071f4d0d9256b40a6bf700b38c6a25db1b26
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2020
ms.locfileid: "88715729"
---
# <a name="azure-stream-analytics-internal-error-codes"></a>Azure 流分析内部错误代码

可以使用活动日志和资源日志来帮助调试 Azure 流分析作业中的意外行为。 本文列出了每个内部错误代码的说明。 内部错误是指当流分析无法区分错误是内部可用性错误还是系统中的 bug 时，流分析平台内引发的一般性错误。

## <a name="cosmosdboutputbatchsizetoolarge"></a>CosmosDBOutputBatchSizeTooLarge

* **原因**：用于写入 Cosmos DB 的批大小太大。 
* **建议**：使用较小的批大小重试。

## <a name="next-steps"></a>后续步骤

* [对输入连接进行故障排除](stream-analytics-troubleshoot-input.md)
* [对 Azure 流分析输出进行故障排除](stream-analytics-troubleshoot-output.md)
* [对 Azure 流分析查询进行故障排除](stream-analytics-troubleshoot-query.md)
* [Azure 流分析数据错误](data-errors.md)

