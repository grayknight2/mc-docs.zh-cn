---
title: 更新 Azure Cosmos DB 的 Gremlin 数据库和图的 RU/秒
description: 更新 Azure Cosmos DB 的 Gremlin 数据库和图的 RU/秒
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
origin.date: 07/29/2020
ms.date: 08/17/2020
ms.testscope: yes
ms.testdate: 08/10/2020
ms.author: v-yeche
ms.openlocfilehash: 97804696299bee7a90383a05c2fe6694be1338e3
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88222559"
---
<!--Verify successfully-->
# <a name="update-rus-for-a-gremlin-database-and-graph-for-azure-cosmos-db-using-azure-cli"></a>使用 Azure CLI 更新 Azure Cosmos DB 的 Gremlin 数据库和图的 RU/秒

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

选择在本地安装并使用 CLI 时，本主题要求运行 Azure CLI 2.9.1 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

此脚本创建一个具有共享吞吐量的 Gremlin 数据库和一个具有专用吞吐量的 Gremlin 图，然后更新该数据库和该图的吞吐量。

```azurecli
#!/bin/bash

# Sign in the Azure China Cloud
az cloud set -n AzureChinaCloud
az login

# Throughput operations for a SQL API database and container

# Generate a unique 10 character alphanumeric string to ensure unique resource names
uniqueId=$(env LC_CTYPE=C tr -dc 'a-z0-9' < /dev/urandom | fold -w 10 | head -n 1)

# Variables for Gremlin API resources
resourceGroupName="Group-$uniqueId"
location='chinanorth2'
accountName="cosmos-$uniqueId" #needs to be lower case
databaseName='database1'
graphName='graph1'
originalThroughput=400
updateThroughput=500

# Create a resource group, Cosmos account, database with throughput and graph with throughput
az group create -n $resourceGroupName -l $location
az cosmosdb create -n $accountName -g $resourceGroupName --capabilities EnableGremlin
az cosmosdb gremlin database create -a $accountName -g $resourceGroupName -n $databaseName --throughput $originalThroughput
az cosmosdb gremlin graph create -a $accountName -g $resourceGroupName -d $databaseName -n $graphName -p '/zipcode' --throughput $originalThroughput

# Throughput operations for Gremlin API database
# Read the current throughput
# Read the minimum throughput
# Make sure the updated throughput is not less than the minimum
# Update the throughput

read -p 'Press any key to read current provisioned throughput on database'

az cosmosdb gremlin database throughput show \
    -g $resourceGroupName \
    -a $accountName \
    -n $databaseName \
    --query resource.throughput \
    -o tsv

read -p 'Press any key to read minimum throughput on database'

minimumThroughput=$(az cosmosdb gremlin database throughput show \
    -g $resourceGroupName \
    -a $accountName \
    -n $databaseName \
    --query resource.minimumThroughput \
    -o tsv)

echo $minimumThroughput

# Make sure the updated throughput is not less than the minimum allowed throughput
if [ $updateThroughput -lt $minimumThroughput ]; then
    updateThroughput=$minimumThroughput
fi

read -p 'Press any key to update database throughput'

az cosmosdb gremlin database throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    --throughput $updateThroughput

# Throughput operations for Gremlin API graph
# Read the current throughput
# Read the minimum throughput
# Make sure the updated throughput is not less than the minimum
# Update the throughput

read -p 'Press any key to read current provisioned throughput on a graph'

az cosmosdb gremlin graph throughput show \
    -g $resourceGroupName \
    -a $accountName \
    -d $databaseName \
    -n $graphName \
    --query resource.throughput \
    -o tsv

read -p 'Press any key to read minimum throughput on graph'

minimumThroughput=$(az cosmosdb gremlin graph throughput show \
    -g $resourceGroupName \
    -a $accountName \
    -d $databaseName \
    -n $graphName \
    --query resource.minimumThroughput \
    -o tsv)

echo $minimumThroughput

# Make sure the updated throughput is not less than the minimum allowed throughput
if [ $updateThroughput -lt $minimumThroughput ]; then
    updateThroughput=$minimumThroughput
fi

az cosmosdb gremlin graph throughput update \
    -g $resourceGroupName \
    -a $accountName \
    -d $databaseName \
    -n $graphName \
    --throughput $updateThroughput

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
| [az cosmosdb gremlin database create](https://docs.azure.cn/cli/cosmosdb/gremlin/database?view=azure-cli-latest#az-cosmosdb-gremlin-database-create) | 创建 Azure Cosmos Gremlin 数据库。 |
| [az cosmosdb gremlin graph create](https://docs.azure.cn/cli/cosmosdb/gremlin/graph?view=azure-cli-latest#az-cosmosdb-gremlin-graph-create) | 创建 Azure Cosmos Gremlin 图。 |
| [az cosmosdb gremlin database throughput update](https://docs.azure.cn/cli/cosmosdb/gremlin/database/throughput?view=azure-cli-latest#az-cosmosdb-gremlin-database-throughput-update) | 更新 Azure Cosmos Gremlin 数据库的 RU/秒。 |
| [az cosmosdb gremlin graph throughput update](https://docs.azure.cn/cli/cosmosdb/gremlin/graph/throughput?view=azure-cli-latest#az-cosmosdb-gremlin-graph-throughput-update) | 更新 Azure Cosmos Gremlin 图的 RU/秒。 |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure Cosmos DB CLI 的详细信息，请参阅 [Azure Cosmos DB CLI 文档](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest)。

可以在 [Azure Cosmos DB CLI GitHub 存储库](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)中找到所有 Azure Cosmos DB CLI 脚本示例。

<!-- Update_Description: update meta properties, wording update, update link -->