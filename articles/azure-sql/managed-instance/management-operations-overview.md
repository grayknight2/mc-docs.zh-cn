---
title: 管理操作
titleSuffix: Azure SQL Managed Instance
description: 了解 Azure SQL 托管实例管理操作的持续时间和最佳做法。
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: sstein, carlrab, MashaMSFT
origin.date: 07/10/2020
ms.date: 08/17/2020
ms.openlocfilehash: 2028a76de240ff7bde901417b65758f370b8c3d0
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223602"
---
# <a name="overview-of-azure-sql-managed-instance-management-operations"></a>Azure SQL 托管实例管理操作概述
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

## <a name="what-are-management-operations"></a>什么是管理操作？
Azure SQL 托管实例提供管理操作，你可以使用这些操作来自动部署新的托管实例、更新实例属性，以及删除不再需要的实例。

为了支持 [Azure 虚拟网络中的部署](../../virtual-network/virtual-network-for-azure-services.md)并为客户提供隔离和安全性，SQL 托管实例依赖于[虚拟群集](connectivity-architecture-overview.md#high-level-connectivity-architecture)。 虚拟群集表示在客户的虚拟网络子网中部署的一组专用的独立虚拟机。 实质上，每在一个空子网中部署一个托管实例，就会组建一个新的虚拟群集。

对部署的托管实例执行后续操作也可能会影响基础虚拟群集。 这些操作会影响管理操作的持续时间，因为部署更多的虚拟机会附带开销，在规划新的部署或者更新现有托管实例时，需要考虑这种开销。

所有管理操作可分类为：

- 部署实例（创建新实例）。
- 实例更新（更改实例属性，例如 vCore 数或预留存储）。
- 删除实例。

## <a name="management-operations-duration"></a>管理操作持续时间
通常，在虚拟群集上执行的操作花费的时间最长。 在虚拟群集上执行的操作的持续时间各不相同 - 下面是根据现有的服务遥测数据通常可预期到的值：

- 虚拟群集创建：创建是实例管理操作中的一个同步步骤。 **90% 的操作可在 4 小时内完成**。
- 虚拟群集大小调整（扩展或收缩）：扩展是一个同步步骤，而收缩以异步方式执行（不影响实例管理操作的持续时间）。 **90% 的群集扩展操作可在 2.5 小时内完成**。
- 虚拟群集删除：删除是一个异步步骤，但也可以在空的虚拟群集上[手动启动](virtual-cluster-delete.md)，在这种情况下，它将以同步方式执行。 **90% 的虚拟群集删除操作可在 1.5 小时内完成**。

此外，实例管理还可能包括托管数据库上的某个操作，这会导致持续时间变长：

- 附加 Azure 存储中的数据库文件：一个同步步骤，例如计算 (vCore)，或者“常规用途”服务层级中的存储纵向扩展或缩减。 **90% 的此类操作可在 5 分钟内完成**。
- Always On 可用性组种子设定：一个同步步骤，例如计算 (vCore)，或者“业务关键”服务层级中的存储缩放，以及将服务层级从“常规用途”更改为“业务关键”（反之亦然）。 此操作的持续时间与总数据库大小以及当前数据库活动（活动的事务数）成正比。 更新实例时执行数据库活动可能会使总持续时间发生明显的变化。 90% 的此类操作能够以 220 GB/小时或更高的速率执行。

下表汇总了操作及其典型的总持续时间：

|类别  |操作  |长时间运行的分段  |预计持续时间  |
|---------|---------|---------|---------|
|**部署** |空子网中的第一个实例|虚拟群集的创建|90% 的操作可在 4 小时内完成。|
|部署 |非空子网中另一个硬件代系的第一个实例（例如，包含第 4 代实例的子网中的第一个 5 代实例）|虚拟群集的创建*|90% 的操作可在 4 小时内完成。|
|部署 |在空或非空子网中创建包含 4 个 vCore 的第一个实例|虚拟群集的创建**|90% 的操作可在 4 小时内完成。|
|部署 |在非空子网中创建后续实例（第 2 个、第 3 个 ... 实例）|虚拟群集大小调整|90% 的操作可在 2.5 小时内完成。|
|**更新** |实例属性更改（管理员密码、Azure AD 登录名、Azure 混合权益标志）|空值|最长 1 分钟。|
|更新 |增加/缩减实例存储空间（“常规用途”服务层级）|附加数据库文件|90% 的操作可在 5 分钟内完成。|
|更新 |实例存储纵向缩放（“业务关键”服务层级）|- 虚拟群集大小调整<br>- Always On 可用性组种子设定|90% 的操作可在“2.5 小时 + 所有数据库种子设定时间”内完成（220 GB/小时）。|
|更新 |实例计算 (vCore) 纵向缩放（“常规用途”）|- 虚拟群集大小调整<br>- 附加数据库文件|90% 的操作可在 2.5 小时内完成。|
|更新 |实例计算 (vCore) 纵向缩放（“业务关键”）|- 虚拟群集大小调整<br>- Always On 可用性组种子设定|90% 的操作可在“2.5 小时 + 所有数据库种子设定时间”内完成（220 GB/小时）。|
|更新 |实例服务层级更改（从“常规用途”更改为“业务关键”，或反之）|- 虚拟群集大小调整<br>- Always On 可用性组种子设定|90% 的操作可在“2.5 小时 + 所有数据库种子设定时间”内完成（220 GB/小时）。|
|**删除**|实例删除|所有数据库的尾部日志备份|90% 的操作在最多 1 分钟内即可完成。<br>请注意：如果删除了子网中的最后一个实例，此操作将计划在 12 小时后删除虚拟群集。**|
|删除|虚拟群集的删除（用户启动的操作）|虚拟群集的删除|90% 的操作可在最多 1.5 小时内可完成。|

\* 虚拟群集是按硬件代系构建的。

\*\* 12 小时是当前配置，但将来可能会更改，因此，请不要过度依赖于此配置。 如果需要提前删除虚拟群集（如释放子网），请参阅[在删除托管实例后删除子网](virtual-cluster-delete.md)。

## <a name="instance-availability-during-management-operations"></a>管理操作期间的实例可用性

SQL 托管实例在更新操作期间可用，但因更新结束时发生的故障转移而导致的短暂故障时间除外。 借助[加速数据库恢复](../accelerated-database-recovery.md)，即使长时间运行的事务中断，此故障时间通常也不会超过 10 秒。

> [!IMPORTANT]
> 不建议在执行长时间运行事务（数据导入、数据处理作业、索引重新生成等）的同时缩放 Azure SQL 托管实例的计算或存储，也不建议同时更改服务层级。 在操作结束时执行的数据库故障转移将取消所有正在进行的事务。

在执行部署和删除操作期间，客户端应用程序无法使用 SQL 托管实例。

## <a name="management-operations-cross-impact"></a>管理操作交叉影响

对托管实例执行管理操作可能会影响放置在同一虚拟群集中的实例的其他管理操作：

- 虚拟群集中长时间运行的还原操作将使同一子网中的其他实例创建或缩放操作暂停。<br/>**示例：** 如果存在长时间运行的还原操作，且同一子网中存在创建或缩放请求，则此请求将需要更长的时间才能完成，因为它将等待还原操作完成后才会继续。
    
- 后续实例创建或缩放操作会因之前启动的实例创建或实例缩放（启动虚拟群集大小调整）而暂停。<br/>**示例：** 如果同一虚拟群集下的同一子网中存在多个创建和/或缩放请求，且其中一个请求启动虚拟群集大小调整，则在提交要求进行虚拟群集大小调整的请求 5 分钟后提交的所有请求的持续时间将比预期要长，因为这些请求必须等待大小调整完成才能恢复运行。

- 在 5 分钟时间范围内提交的创建/缩放操作将以并行方式批处理和执行。<br/>**示例：** 对于在 5 分钟时间范围内提交的所有操作（从执行第一个操作请求的那一刻开始计算），只会执行一次虚拟群集大小调整。 如果另一个请求在提交第一个请求 5 分钟后才提交，则该请求将等待虚拟群集大小调整完成后才会开始执行。

> [!IMPORTANT]
> 满足继续执行的条件后，因另一个操作正在进行而被暂停的管理操作将自动恢复运行。 无需用户执行任何操作即可恢复临时暂停的管理操作。

## <a name="canceling-management-operations"></a>取消管理操作

下表汇总了取消特定管理操作的功能和典型的总持续时间：

Category  |操作  |可取消  |估计取消持续时间  |
|---------|---------|---------|---------|
|部署 |实例创建 |否 |  |
|更新 |实例存储纵向缩放（“常规用途”服务层级） |否 |  |
|更新 |实例存储纵向缩放（“业务关键”服务层级） |是 |90% 的操作可在 5 分钟内完成。 |
|更新 |增加和减少实例计算大小 (vCore)（常规用途） |是 |90% 的操作可在 5 分钟内完成。 |
|更新 |增加和减少实例计算大小 (vCore)（业务关键） |是 |90% 的操作可在 5 分钟内完成。 |
|更新 |更改实例服务层级（在“常规用途”和“业务关键”之间更改） |是 |90% 的操作可在 5 分钟内完成。 |
|Delete |实例删除 |否 |  |
|删除 |删除虚拟群集（作为用户启动的操作） |否 |  |

# <a name="portal"></a>[门户](#tab/azure-portal)

若要取消管理操作，请转到“概述”边栏选项卡，单击正在进行的操作的通知框。 右侧会显示包含正在进行的操作的屏幕，并提供用于取消操作的按钮。 第一次单击该按钮后，系统会要求再次单击并确认取消该操作。

[![取消操作](./media/management-operations-overview/canceling-operation.png)](./media/management-operations-overview/canceling-operation.png#lightbox)

提交并处理取消请求后，你会收到通知，指出取消提交是否成功。

如果提交的取消请求成功，则管理操作将在几分钟内取消，从而导致失败。

![取消操作结果](./media/management-operations-overview/canceling-operation-result.png)

如果取消请求失败或取消按钮处于非活动状态，这表示管理操作已进入不可取消状态，将在几分钟内完成。 管理操作将继续执行，直到完成。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

如果尚未安装 Azure PowerShell，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。

若要取消管理操作，需要指定管理操作名称。 因此，首先使用 get 命令来检索操作列表，然后取消特定操作。

```powershell
$managedInstance = "yourInstanceName"
$resourceGroup = "yourResourceGroupName"

$managementOperations = Get-AzSqlInstanceOperation -ManagedInstanceName $managedInstance  -ResourceGroupName $resourceGroup

foreach ($mo in $managementOperations ) {
    if($mo.State -eq "InProgress" -and $mo.IsCancellable){
        $cancelRequest = Stop-AzSqlInstanceOperation -ResourceGroupName $resourceGroup -ManagedInstanceName $managedInstance -Name $mo.Name
        Get-AzSqlInstanceOperation -ManagedInstanceName $managedInstance  -ResourceGroupName $resourceGroup -Name $mo.Name
    }
}
```

有关详细的命令说明，请参阅 [Get-AzSqlInstanceOperation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstanceoperation) 和 [Stop-AzSqlInstanceOperation](https://docs.microsoft.com/powershell/module/az.sql/stop-azsqlinstanceoperation)。

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

如果尚未安装 Azure CLI，请参阅[安装 Azure CLI](/cli/install-azure-cli?view=azure-cli-latest)。

若要取消管理操作，需要指定管理操作名称。 因此，首先使用 get 命令来检索操作列表，然后取消特定操作。

```azurecli
az sql mi op list -g yourResourceGroupName --mi yourInstanceName --query "[?state=='InProgress' && isCancellable].{Name: name}" -o tsv |
while read -r operationName; do
    az sql mi op cancel -g yourResourceGroupName --mi yourInstanceName -n $operationName
done
```

有关详细的命令说明，请参阅 [az sql mi op](/cli/sql/mi/op?view=azure-cli-latest)。

---

## <a name="next-steps"></a>后续步骤
- 若要了解如何创建第一个托管实例，请参阅[快速入门指南](instance-create-quickstart.md)。
- 有关功能和比较列表，请参阅 [SQL 常用功能](../database/features-comparison.md)。
- 有关 VNet 配置的详细信息，请参阅 [SQL 托管实例 VNet 配置](connectivity-architecture-overview.md)。
- 有关创建托管实例以及从备份文件还原数据库的快速入门，请参阅[创建托管实例](instance-create-quickstart.md)。
- 有关使用 Azure 数据库迁移服务进行迁移的教程，请参阅[使用数据库迁移服务进行 SQL 托管实例迁移](../../dms/tutorial-sql-server-to-managed-instance.md)。
