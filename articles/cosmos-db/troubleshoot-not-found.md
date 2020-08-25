---
title: 排查 Azure Cosmos DB 未找到异常
description: 如何诊断和修复未找到异常
author: rockboyfor
ms.service: cosmos-db
origin.date: 07/13/2020
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 10c6b052eb85d29a3ee109d759c27bd4262c2983
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223482"
---
<!--Verified successfully-->
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-not-found"></a>诊断和排查 Azure Cosmos DB 未找到异常
HTTP 状态代码 404 表示资源不再存在。

## <a name="expected-behavior"></a>预期行为
有许多应用程序预期接收 404 并可正确进行处理的有效方案。

## <a name="not-found-was-returned-for-an-item-that-should-exist-or-does-exist"></a>对应该存在或确实存在的项返回“未找到”
如果项应该或确实存在，则以下是返回状态代码 404 的可能原因。

### <a name="1-race-condition"></a>1.争用条件
有多个 SDK 客户端实例且读取在写入之前发生。

#### <a name="solution"></a>解决方案：
1. Cosmos DB 的默认帐户一致性为会话一致性。 创建或更新项时，响应将返回一个会话令牌，其可以在 SDK 实例之间传递，以确保读取请求在从具有该更改的副本中进行读取。
2. 将[一致性级别](consistency-levels-choosing.md)更改为[更高级别](consistency-levels-tradeoffs.md)

### <a name="2-invalid-partition-key-and-id-combination"></a>2.分区键和 ID 组合无效
分区键和 ID 组合无效。

#### <a name="solution"></a>解决方案：
修复导致错误组合的应用程序逻辑。 

### <a name="3-invalid-character-in-item-id"></a>3.项 ID 中的字符无效
项被插入 Cosmos DB，并且项 ID 中带有[无效字符](https://docs.azure.cn/dotnet/api/microsoft.azure.documents.resource.id?view=azure-dotnet#remarks)。

#### <a name="solution"></a>解决方案：
建议用户将 ID 更改为不包含特殊字符的其他值。 如果不能更改 ID，则可以对 ID 进行 Base64 编码以转义特殊字符。

对于已经插入容器中的项，可以使用 RID 值来替换 ID，而不使用基于名称的引用。
```c#
// Get a container reference that use RID values
ContainerProperties containerProperties = await this.Container.ReadContainerAsync();
string[] selfLinkSegments = containerProperties.SelfLink.Split('/');
string databaseRid = selfLinkSegments[1];
string containerRid = selfLinkSegments[3];
Container containerByRid = this.cosmosClient.GetContainer(databaseRid, containerRid);

// List of invalid characters are listed here.
//https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.resource.id?view=azure-dotnet#remarks
FeedIterator<JObject> invalidItemsIterator = this.Container.GetItemQueryIterator<JObject>(
    @"select * from t where CONTAINS(t.id, ""/"") or CONTAINS(t.id, ""#"") or CONTAINS(t.id, ""?"") or CONTAINS(t.id, ""\\"") ");
while (invalidItemsIterator.HasMoreResults)
{
    foreach (JObject itemWithInvalidId in await invalidItemsIterator.ReadNextAsync())
    {
        // It recommend to chose a new ID that does not contain special characters, but
        // if that is not possible then it can be Base64 encoded to escape the special characters
        byte[] plainTextBytes = Encoding.UTF8.GetBytes(itemWithInvalidId["id"].ToString());
        itemWithInvalidId["id"] = Convert.ToBase64String(plainTextBytes);

        // Update the item with the new ID value using the rid based container reference
        JObject item = await containerByRid.ReplaceItemAsync<JObject>(
            item: itemWithInvalidId,
            ID: itemWithInvalidId["_rid"].ToString(),
            partitionKey: new Cosmos.PartitionKey(itemWithInvalidId["status"].ToString()));

        // Validate the new ID can be read using the original name based contianer reference
        await this.Container.ReadItemAsync<ToDoActivity>(
            item["id"].ToString(),
            new Cosmos.PartitionKey(item["status"].ToString())); ;
    }
}
```

### <a name="4-time-to-live-ttl-purge"></a>4.生存时间 (TTL) 清除
项已设置[生存时间 (TTL)](/cosmos-db/time-to-live) 属性。 由于生存时间已过期，项被清除。

#### <a name="solution"></a>解决方案：
更改生存时间以防止项被清除。

### <a name="5-lazy-indexing"></a>5.惰性索引编制
[惰性索引编制](index-policy.md#indexing-mode)未跟进。

#### <a name="solution"></a>解决方案：
等待索引编制跟进或更改索引编制策略

### <a name="6-parent-resource-deleted"></a>6.已删除父资源
项所在的数据库和/或容器已删除。

#### <a name="solution"></a>解决方案：
1. [还原](/cosmos-db/online-backup-and-restore#backup-retention-period)父资源或重新创建资源。
2. 创建新资源来替换已删除的资源

## <a name="next-steps"></a>后续步骤
* [诊断和排查](troubleshoot-dot-net-sdk.md)使用 Azure Cosmos DB .NET SDK 时遇到的问题
* 了解 [.NET V3](performance-tips-dotnet-sdk-v3-sql.md) 和 [.NET V2](performance-tips.md) 的性能准则

<!-- Update_Description: new article about troubleshoot not found -->
<!--NEW.date: 08/10/2020-->