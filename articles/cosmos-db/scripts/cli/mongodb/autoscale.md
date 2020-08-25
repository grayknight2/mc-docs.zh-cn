---
title: 为 MongoDB API for Azure Cosmos DB 创建数据库（具有自动缩放功能和共享集合）
description: 为 MongoDB API for Azure Cosmos DB 创建数据库（具有自动缩放功能和共享集合）
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
origin.date: 07/29/2020
ms.date: 08/17/2020
ms.testscope: yes
ms.testdate: 08/10/2020
ms.author: v-yeche
ms.openlocfilehash: 8ad1957af5774f5db42ab94f2f5868b11e8fb95c
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223500"
---
<!--Verify successfully-->
# <a name="create-a-database-with-autoscale-and-shared-collections-for-mongodb-api-for-azure-cosmos-db-using-azure-cli"></a>使用 Azure CLI 为 MongoDB API for Azure Cosmos DB 创建数据库（具有自动缩放功能和共享集合）

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

选择在本地安装并使用 CLI 时，本主题要求运行 Azure CLI 2.9.1 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Create a MongoDB API database with autoscale and 2 collections that share throughput

# Generate a unique 10 character alphanumeric string to ensure unique resource names
uniqueId=$(env LC_CTYPE=C tr -dc 'a-z0-9' < /dev/urandom | fold -w 10 | head -n 1)

# Variables for MongoDB API resources
resourceGroupName="Group-$uniqueId"
location='chinanorth2'
accountName="cosmos-$uniqueId" #needs to be lower case
serverVersion='3.6' #3.2 or 3.6
databaseName='database1'
maxThroughput=4000 #minimum = 4000
collection1Name='collection1'
collection2Name='collection2'

# Create a resource group
az group create -n $resourceGroupName -l $location

# Create a Cosmos account for MongoDB API
az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --kind MongoDB \
    --server-version $serverVersion \
    --default-consistency-level Eventual \
    --locations regionName='China North 2' failoverPriority=0 isZoneRedundant=False

# Create a MongoDB API database with shared autoscale throughput
az cosmosdb mongodb database create \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    --max-throughput $maxThroughput

# Create two MongoDB API collections to share throughput
az cosmosdb mongodb collection create \
    -a $accountName \
    -g $resourceGroupName \
    -d $databaseName \
    -n $collection1Name \
    --shard 'myShardKey1'

az cosmosdb mongodb collection create \
    -a $accountName \
    -g $resourceGroupName \
    -d $databaseName \
    -n $collection2Name \
    --shard 'myShardKey2'

```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```azurecli
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az cosmosdb create](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest#az-cosmosdb-create) | 创建 Azure Cosmos DB 帐户。 |
| [az cosmosdb mongodb database create](https://docs.azure.cn/cli/cosmosdb/mongodb/database?view=azure-cli-latest#az-cosmosdb-mongodb-database-create) | 创建 Azure Cosmos MongoDB API 数据库。 |
| [az cosmosdb mongodb collection create](https://docs.azure.cn/cli/cosmosdb/mongodb/collection?view=azure-cli-latest#az-cosmosdb-mongodb-collection-create) | 创建 Azure Cosmos MongoDB API 集合。 |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure Cosmos DB CLI 的详细信息，请参阅 [Azure Cosmos DB CLI 文档](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest)。

可以在 [Azure Cosmos DB CLI GitHub 存储库](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)中找到所有 Azure Cosmos DB CLI 脚本示例。

<!-- Update_Description: new article about autoscale -->
<!--NEW.date: 08/10/2020-->