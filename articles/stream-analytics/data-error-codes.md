---
title: 数据错误代码 - Azure 流分析
description: 通过数据错误代码对 Azure 流分析进行故障排除。
ms.author: v-johya
author: Johnnytechn
ms.topic: conceptual
ms.date: 07/06/2020
ms.service: stream-analytics
ms.openlocfilehash: 0b3ca64ffc5ace393e8b71a7355f587102b0a139
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226285"
---
# <a name="azure-stream-analytics-configuration-error-codes"></a>Azure 流分析配置错误代码

可以使用活动日志和资源日志来帮助调试 Azure 流分析作业中的意外行为。 本文列出了每个数据错误代码的说明。 当流中存在错误数据（例如意外的记录架构），就会出现数据错误。

## <a name="inputdeserializationerror"></a>InputDeserializationError

* **原因**：在对输入数据进行反序列化时出错。

## <a name="inputeventtimestampnotfound"></a>InputEventTimestampNotFound

* **原因**：流分析无法获得资源的时间戳。 

## <a name="inputeventtimestampbyovervaluenotfound"></a>InputEventTimestampByOverValueNotFound

* **原因**：流分析无法获取 `TIMESTAMP BY OVER COLUMN` 的值。

## <a name="inputeventlatebeyondthreshold"></a>InputEventLateBeyondThreshold

* **原因**：输入事件的发送时间晚于配置的容差值。

## <a name="inputeventearlybeyondthreshold"></a>InputEventEarlyBeyondThreshold

* **原因**：输入事件到达时间早于输入事件应用程序时间戳阈值。

## <a name="azurefunctionmessagesizeexceeded"></a>AzureFunctionMessageSizeExceeded

* **原因**：输出到 Azure Functions 的消息超过了大小限制。

## <a name="eventhuboutputrecordexceedssizelimit"></a>EventHubOutputRecordExceedsSizeLimit

* **原因**：输出记录超出了将数据写入到事件中心时的最大大小限制。

## <a name="cosmosdboutputinvalidid"></a>CosmosDBOutputInvalidId

* **原因**：特定列的值或类型无效。
* **建议**：提供长度不超过 255 个字符的唯一非空字符串。

## <a name="cosmosdboutputinvalididcharacter"></a>CosmosDBOutputInvalidIdCharacter

* **原因**：输出记录的文档 ID 包含无效字符。

## <a name="cosmosdboutputmissingid"></a>CosmosDBOutputMissingId

* **原因**：输出记录中没有要用作主键属性的列 \[id]。

## <a name="cosmosdboutputmissingidcolumn"></a>CosmosDBOutputMissingIdColumn

* **原因**：输出记录不包含文档 ID 属性。 
* **建议**：确保查询输出包含具有长度小于“255”字符的唯一非空字符串的列。

## <a name="cosmosdboutputmissingpartitionkey"></a>CosmosDBOutputMissingPartitionKey

* **原因**：输出记录缺少要用作分区键属性的列。

## <a name="cosmosdboutputsinglerecordtoolarge"></a>CosmosDBOutputSingleRecordTooLarge

* **原因**：写入到 Cosmos DB 的一个记录过大。

## <a name="sqldatabaseoutputdataerror"></a>SQLDatabaseOutputDataError

* **原因**：流分析无法将一个或多个事件写入 SQL 数据库，原因是数据存在问题。

## <a name="next-steps"></a>后续步骤

* [对输入连接进行故障排除](stream-analytics-troubleshoot-input.md)
* [对 Azure 流分析输出进行故障排除](stream-analytics-troubleshoot-output.md)
* [对 Azure 流分析查询进行故障排除](stream-analytics-troubleshoot-query.md)
* [Azure 流分析数据错误](data-errors.md)

