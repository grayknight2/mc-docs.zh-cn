---
title: 如何提高复原能力
titleSuffix: Azure Machine Learning
description: 了解如何通过使用高可用性配置提高与 Azure 机器学习相关的资源的中断复原能力。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jhirono
author: jhirono
ms.reviewer: larryfr
ms.date: 07/16/2020
ms.openlocfilehash: 5f72a6b048eac000e033284aecc7f6351e740fc1
ms.sourcegitcommit: 9d9795f8a5b50cd5ccc19d3a2773817836446912
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88228987"
---
# <a name="increase-the-resiliency-of-azure-machine-learning"></a>提高 Azure 机器学习的复原能力

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

了解如何通过使用高可用性配置提高与 Azure 机器学习相关的资源的复原能力。 可以配置 Azure 机器学习所依赖的 Azure 服务以实现高可用性。 本文提供有关可以配置哪些服务以实现高可用性的信息，并提供有关如何配置这些资源的信息的链接。

> [!NOTE]
> Azure 机器学习本身不提供灾难恢复选项。

## <a name="understand-azure-services-for-azure-machine-learning"></a>了解适用于 Azure 机器学习的 Azure 服务

Azure 机器学习依赖于多个 Azure 服务，并具有多个层。 其中一些服务已在（客户）订阅中预配。 你对这些服务的高可用性配置负责。 还有一些服务在 Microsoft 订阅中创建，并由 Microsoft 管理。

* **Azure 机器学习基础结构**：适用于 Azure 机器学习工作区的 Microsoft 托管环境。

* **关联资源**：Azure 机器学习工作区创建期间在订阅中预配的资源。 它们包括 Azure 存储、Azure 密钥保管库、Azure 容器注册表 (ACR) 和 App Insights。 你对这些资源的高可用性设置负责。
  * 默认存储具有模型、训练日志数据和数据集等数据。
  * 密钥保管库具有用于存储、ACR 和数据存储的凭据。
  * ACR 具有用于训练和推理环境的 docker 映像。
  * App Insights 用于监视 Azure 机器学习。

* **计算资源**：在部署工作区之后创建的资源。 例如，可能会创建一个计算实例或计算群集来训练机器学习模型。
  * 计算实例和计算群集：Microsoft 托管模型开发环境。
  * 其他资源：它们是可以附加到 Azure 机器学习的计算资源，例如 Azure Kubernetes 服务 (AKS)、Azure Databricks、Azure 容器实例 (ACI) 和 HDInsight。 你对高可用性设置负责。

* **其他数据存储**：Azure 机器学习可以装载其他数据存储（例如 Azure 存储、Azure Data Lake Storage 和 Azure SQL 数据库）用于训练数据。  它们位于订阅中，你对高可用性设置负责。

下表显示由 Microsoft 管理的服务、由你管理的服务以及默认具有高可用性的服务：

| 服务 | 管理者 | 默认为 HA |
| ----- | ----- | ----- |
| **Azure 机器学习基础结构** | Microsoft | |
| **关联资源** |
| Azure 存储 | 你 | |
| Azure Key Vault | 你 | ✓ |
| Azure 容器注册表 | 你 | |
| Application Insights | 你 | NA |
| **计算资源** |
| 计算实例 | Microsoft |  |
| 计算群集 | Microsoft |  |
| 其他资源，例如 Azure Kubernetes 服务、 <br>Azure Databricks、Azure 容器实例、Azure HDInsight | 你 |  |
| **其他数据存储**，例如 Azure 存储、Azure SQL 数据库、<br> Azure Database for PostgreSQL、Azure Database for MySQL、 <br>Azure Databricks 文件系统 | 你 | |

使用本文档其余部分中的信息，了解为使其中每项服务都保持高可用性而需要采取的措施。

## <a name="associated-resources"></a>关联资源

> [!IMPORTANT]
> Azure 机器学习不支持使用异地冗余存储 (GRS)、异地区域冗余存储 (GZRS)、读取访问异地冗余存储 (RA-GRS) 或读取访问异地区域冗余存储 (RA-GZRS) 的默认存储帐户故障转移。

使用以下信息确保配置每个资源的高可用性设置。

* **Azure 存储**：若要配置高可用性设置，请参阅 [Azure 存储冗余](https://docs.microsoft.com/azure/storage/common/storage-redundancy)。
* **Azure Key Vault**：它提供默认高可用性服务，无需用户采取任何措施。  请参阅 [Azure 密钥保管库可用性和冗余](https://docs.microsoft.com/azure/key-vault/general/disaster-recovery-guidance)。
* **Azure 容器注册表**：选择高级 SKU 进行异地复制。 请参阅 [Azure 容器注册表中的异地复制](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication)。
* **Application Insights**：它不提供高可用性设置。 可以在 [Application Insights 中的数据收集、保留和存储](https://docs.microsoft.com/azure/azure-monitor/app/data-retention-privacy#how-long-is-the-data-kept)中调整数据保留期和详细信息。

## <a name="compute-resources"></a>计算资源

使用以下文档确保配置每个资源的高可用性设置。

* **Azure Kubernetes 服务**：请参阅 [Azure Kubernetes 服务 (AKS) 中实现业务连续性和灾难恢复的最佳做法](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region)和[创建使用可用性区域的 Azure Kubernetes 服务 (AKS) 群集](https://docs.microsoft.com/azure/aks/availability-zones)。 如果 AKS 群集由 Azure 机器学习创建（使用 Studio、SDK 或 ML CLI），则不支持跨区域高可用性。
* **Azure Databricks**：请参阅 [Azure Databricks 群集的区域性灾难恢复](https://docs.microsoft.com/azure/azure-databricks/howto-regional-disaster-recovery)。
* **Azure 容器实例**：ACI 业务流程协调程序对故障转移负责。 请参阅 [Azure 容器实例和容器业务流程协调程序](https://docs.microsoft.com/azure/container-instances/container-instances-orchestrator-relationship)。
* **Azure HDInsight**：请参阅 [Azure HDInsight 支持的高可用性服务](https://docs.microsoft.com/azure/hdinsight/hdinsight-high-availability-components)。

## <a name="additional-data-stores"></a>其他数据存储

使用以下文档确保配置每个资源的高可用性设置。

* **Azure Blob 容器/Azure 文件共享/Azure Data Lake Gen2**：与默认存储相同。
* **Azure Data Lake Gen1**：请参阅 [Azure Data Lake Storage Gen1 数据灾难恢复指南](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-disaster-recovery-guidance)。
* **Azure SQL 数据库**：请参阅[高可用性和 Azure SQL 数据库](https://docs.microsoft.com/azure/sql-database/sql-database-high-availability)。
* **Azure Database for PostgreSQL**：请参阅 [Azure Database for PostgreSQL - 单一服务器中的高可用性概念](https://docs.microsoft.com/azure/postgresql/concepts-high-availability)。
* **Azure Database for MySQL**：请参阅[了解 Azure Database for MySQL 中的业务连续性](https://docs.microsoft.com/azure/mysql/concepts-business-continuity)。
* **Databricks 文件系统**：请参阅 [Azure Databricks 群集的区域性灾难恢复](https://docs.microsoft.com/azure/azure-databricks/howto-regional-disaster-recovery)。

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

如果提供自己的密钥（客户管理的密钥）来部署 Azure 机器学习工作区，则还会在订阅中配预配 Cosmos DB。 在这种情况下，你应对其高可用性负责。 请参阅[使用 Azure Cosmos DB 实现高可用性](https://docs.microsoft.com/azure/cosmos-db/high-availability)

## <a name="next-steps"></a>后续步骤

若要使用具有高可用性设置的关联资源部署 Azure 机器学习，请使用 [Azure 资源管理器模板](https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced)。