---
title: 创建 Azure Cosmos DB 的 Gremlin 数据库和图
description: 创建 Azure Cosmos DB 的 Gremlin 数据库和图
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
origin.date: 07/29/2020
ms.date: 08/17/2020
ms.testscope: yes
ms.testdate: 08/10/2020
ms.author: v-yeche
ms.openlocfilehash: 90a97c935ddc13e0d015b030cbf56d610cdd63dc
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88222560"
---
<!--Verify successfully-->
# <a name="create-an-azure-cosmos-gremlin-api-account-database-and-graph-using-azure-cli"></a>使用 Azure CLI 创建 Azure Cosmos Gremlin API 帐户、数据库和图

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

选择在本地安装并使用 CLI 时，本主题要求运行 Azure CLI 2.9.1 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Sign in the Azure China Cloud
az cloud set -n AzureChinaCloud
az login

# Create a Gremlin database and graph

# Generate a unique 10 character alphanumeric string to ensure unique resource names
uniqueId=$(env LC_CTYPE=C tr -dc 'a-z0-9' < /dev/urandom | fold -w 10 | head -n 1)

# Variables for Gremlin API resources
resourceGroupName="Group-$uniqueId"
location='chinanorth2'
accountName="cosmos-$uniqueId" #needs to be lower case
databaseName='database1'
graphName='graph1'

# Create a resource group
az group create -n $resourceGroupName -l $location

# Create a Cosmos account for Gremlin API
az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --capabilities EnableGremlin \
    --default-consistency-level Eventual \
    --locations regionName='China North 2' failoverPriority=0 isZoneRedundant=False \
    --locations regionName='China East 2' failoverPriority=1 isZoneRedundant=False

# Create a Gremlin database
az cosmosdb gremlin database create \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName

# Define the index policy for the graph, include spatial and composite indexes
idxpolicy=$(cat << EOF 
{
    "indexingMode": "consistent", 
    "includedPaths": [
        {"path": "/*"}
    ],
    "excludedPaths": [
        { "path": "/headquarters/employees/?"}
    ],
    "spatialIndexes": [
        {"path": "/*", "types": ["Point"]}
    ],
    "compositeIndexes":[
        [
            { "path":"/name", "order":"ascending" },
            { "path":"/age", "order":"descending" }
        ]
    ]
}
EOF
)
# Persist index policy to json file
echo "$idxpolicy" > "idxpolicy-$uniqueId.json"

# Create a Gremlin graph
az cosmosdb gremlin graph create \
    -a $accountName \
    -g $resourceGroupName \
    -d $databaseName \
    -n $graphName \
    -p '/zipcode' \
    --throughput 400 \
    --idx @idxpolicy-$uniqueId.json

# Clean up temporary index policy file
rm -f "idxpolicy-$uniqueId.json"

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
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure Cosmos DB CLI 的详细信息，请参阅 [Azure Cosmos DB CLI 文档](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest)。

可以在 [Azure Cosmos DB CLI GitHub 存储库](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)中找到所有 Azure Cosmos DB CLI 脚本示例。

<!-- Update_Description: update meta properties, wording update, update link -->