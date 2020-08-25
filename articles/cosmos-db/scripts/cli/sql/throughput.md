---
title: 更新 Azure Cosmos DB 的 SQL (Core) API 数据库和容器的 RU/秒
description: 更新 Azure Cosmos DB 的 SQL (Core) API 数据库和容器的 RU/秒
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
origin.date: 07/29/2020
ms.date: 08/17/2020
ms.testscope: yes
ms.testdate: 08/10/2020
ms.author: v-yeche
ms.openlocfilehash: 4b66d08d8669ae1d942b2e8ced6272c5f808bec8
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223423"
---
<!--Verified successfully-->
# <a name="update-rus-for-a-sql-core-api-database-and-container-for-azure-cosmos-db-using-azure-cli"></a>使用 Azure CLI 更新 Azure Cosmos DB 的 SQL (Core) API 数据库和容器的 RU/秒

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

选择在本地安装并使用 CLI 时，本主题要求运行 Azure CLI 2.9.1 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

此脚本创建一个具有共享吞吐量的 SQL (Core) API 数据库和一个具有专用吞吐量的 SQL (Core) API 容器，然后更新该数据库和该容器的吞吐量。

```azurecli
#!/bin/bash

# Sign in the Azure China Cloud
az cloud set -n AzureChinaCloud
az login

# Throughput operations for a SQL API database and container

# Generate a unique 10 character alphanumeric string to ensure unique resource names
uniqueId=$(env LC_CTYPE=C tr -dc 'a-z0-9' < /dev/urandom | fold -w 10 | head -n 1)
# Variables for SQL API resources
resourceGroupName="Group-$uniqueId"
location='chinanorth2'
accountName="cosmos-$uniqueId" #needs to be lower case
databaseName='database1'
containerName='container1'
originalThroughput=400
updateThroughput=500

# Create a resource group, Cosmos account, database and container
az group create -n $resourceGroupName -l $location
az cosmosdb create -n $accountName -g $resourceGroupName
az cosmosdb sql database create -a $accountName -g $resourceGroupName -n $databaseName --throughput $originalThroughput
az cosmosdb sql container create -a $accountName -g $resourceGroupName -d $databaseName -n $containerName -p '/myPk' --throughput $originalThroughput

# Throughput operations for SQL API database
# Read the current throughput
# Read the minimum throughput
# Make sure the updated throughput is not less than the minimum
# Update the throughput
read -p 'Press any key to read current provisioned throughput on database'

az cosmosdb sql database throughput show \
    -g $resourceGroupName \
    -a $accountName \
    -n $databaseName \
    --query resource.throughput \
    -o tsv

read -p 'Press any key to read minimum throughput on database'

minimumThroughput=$(az cosmosdb sql database throughput show \
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

az cosmosdb sql database throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    --throughput $updateThroughput

# Throughput operations for SQL API container
# Read the current throughput
# Read the minimum throughput
# Make sure the updated throughput is not less than the minimum
# Update the throughput
read -p 'Press any key to read current provisioned throughput on container'

az cosmosdb sql container throughput show \
    -g $resourceGroupName \
    -a $accountName \
    -d $databaseName \
    -n $containerName \
    --query resource.throughput \
    -o tsv

read -p 'Press any key to read minimum throughput on container'

minimumThroughput=$(az cosmosdb sql container throughput show \
    -g $resourceGroupName \
    -a $accountName \
    -d $databaseName \
    -n $containerName \
    --query resource.minimumThroughput \
    -o tsv)

echo $minimumThroughput

# Make sure the updated throughput is not less than the minimum allowed throughput
if [ $updateThroughput -lt $minimumThroughput ]; then
    updateThroughput=$minimumThroughput
fi

read -p 'Press any key to update container throughput'

az cosmosdb sql container throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -d $databaseName \
    -n $containerName \
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
| [az cosmosdb sql database create](https://docs.azure.cn/cli/cosmosdb/sql/database?view=azure-cli-latest#az-cosmosdb-sql-database-create) | 创建 Azure Cosmos SQL (Core) 数据库。 |
| [az cosmosdb sql container create](https://docs.azure.cn/cli/cosmosdb/sql/container?view=azure-cli-latest#az-cosmosdb-sql-container-create) | 创建 Azure Cosmos SQL (Core) 容器。 |
| [az cosmosdb sql database throughput update](https://docs.azure.cn/cli/cosmosdb/sql/database/throughput?view=azure-cli-latest#az-cosmosdb-sql-database-throughput-update) | 更新 Azure Cosmos SQL (Core) 数据库的 RU/秒。 |
| [az cosmosdb sql container throughput update](https://docs.azure.cn/cli/cosmosdb/sql/container/throughput?view=azure-cli-latest#az-cosmosdb-sql-container-throughput-update) | 更新 Azure Cosmos SQL (Core) 容器的 RU/秒。 |
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure Cosmos DB CLI 的详细信息，请参阅 [Azure Cosmos DB CLI 文档](https://docs.azure.cn/cli/cosmosdb?view=azure-cli-latest)。

可以在 [Azure Cosmos DB CLI GitHub 存储库](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)中找到所有 Azure Cosmos DB CLI 脚本示例。

<!-- Update_Description: update meta properties, wording update, update link -->