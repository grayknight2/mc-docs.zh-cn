---
title: 在 Azure Stack Hub 上设置 AKS 引擎的先决条件
description: 确定在 Azure Stack Hub 上运行 ASK 引擎所需的要求。
author: WenJason
ms.topic: article
origin.date: 06/19/2020
ms.date: 07/20/2020
ms.author: v-jay
ms.reviewer: waltero
ms.lastreviewed: 06/19/2020
ms.openlocfilehash: 26467dd5fbcfc42225549359faad5e885bf9cf29
ms.sourcegitcommit: e9ffd50aa5eaab402a94bfabfc70de6967fe6278
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2020
ms.locfileid: "86307431"
---
# <a name="set-up-the-prerequisites-for-the-aks-engine-on-azure-stack-hub"></a>在 Azure Stack Hub 上设置 AKS 引擎的先决条件

可以将 AKS 引擎安装在环境中的虚拟机 (VM) 上，也可以安装在可访问 Azure Stack Hub 资源管理器终结点的任何客户端计算机上。 运行引擎之前，需具备以下各项：AKS 库 Ubuntu 服务器和订阅中可用的 Linux 自定义脚本扩展、已分配给参与者角色的服务主体标识，以及 SSH 用于访问 Ubuntu 服务器的私钥/公钥对。 此外，如果使用的是 Azure Stack 开发工具包，则需要使计算机信任相应的证书。

如果已满足先决条件，则可以开始[定义群集](azure-stack-kubernetes-aks-engine-deploy-cluster.md)。

如果你是 Azure Stack Hub 的云操作员，并且想要提供 AKS 引擎，请按照[将 AKS 引擎添加到 Azure Stack Hub 市场](../operator/azure-stack-aks-engine.md)中的说明进行操作。

## <a name="prerequisites-for-the-aks-engine"></a>AKS 引擎的先决条件

若要使用 AKS 引擎，需具备以下资源。 请记住，Azure Stack Hub 的租户将使用 AKS 引擎在其租户订阅中部署 Kubernetes 群集。 下载市场项目和创建服务主体标识是唯一可能需要 Azure Stack Hub 操作员参与的部分。 可以在下表中找到详细信息。

云操作员需准备好以下各项。

| 先决条件 | 说明 | 必须 | 说明 |
| --- | --- | --- | --- | --- |
| Azure Stack Hub 2002 或更高版本 | AKS 引擎需要 Azure Stack Hub 2002 或更高版本。 | 必须 | 如果不确定你的 Azure Stack Hub 版本是否为最新版本，请联系云操作员。 |
| Linux 自定义脚本扩展 | Linux 自定义脚本扩展 2.0<br>产品/服务：适用于 Linux 的自定义脚本 2.0<br>版本：2.0.6（或最新版本）<br>发布者：Microsoft Corp | 必须 | 如果订阅中没有此项，请联系云操作员。 |
| AKS Ubuntu 基础映像 | AKS 基础 Ubuntu 16.04-LTS 映像发行版 2020 年 5 月 (2020.05.13) | 必须 | 如果订阅中没有此项，请联系云操作员。 有关版本依赖项的详细信息，请参阅[将引擎与基础映像版本相匹配](#matching-engine-to-base-image-version)。<br> 如果你是 Azure Stack Hub 的云操作员，并且想要提供 AKS 引擎，请按照[将 AKS 引擎添加到 Azure Stack Hub 市场](../operator/azure-stack-aks-engine.md)中的说明进行操作。 |
| 服务主体标识 (SPN) |  需要通过 Azure 资源管理器部署或配置资源的应用程序必须以服务主体来表示。 | 必须 | 对于此项，可能需要联系 Azure Stack Hub 操作员。<br>如果使用 Azure Active Directory (AAD) 服务主体标识，则需要能够从 Kubernetes 群集中的 VM 访问 Internet，以便使用 AAD 对服务主体进行身份验证。 如果无法访问 Internet，则 Kubernetes 群集将无法正常运行。<br>有关说明，请参阅[使用应用标识访问资源](/azure-stack/operator/azure-stack-create-service-principals) |
| 分配了 (SPN) 的参与者角色 | 若要允许应用程序使用其服务主体访问订阅中的资源，必须将该服务主体分配到特定资源的某个角色。 | 必须 | 有关说明，请参阅[分配角色](/azure-stack/operator/azure-stack-create-service-principals#assign-a-role) |


可设置以下项。

| 先决条件 | 说明 | 必须 | 说明 |
| --- | --- | --- | --- |
| Azure Stack Hub 订阅 | 可以通过订阅访问 Azure Stack Hub 中的套餐。 套餐包含可供你使用的服务。 | 必须 | 需要先获得 [Azure Stack Hub 订阅](/azure-stack/user/azure-stack-subscribe-services)，才能在 Azure Stack Hub 中部署任何租户工作负载。 |
| 资源组 | 资源组是用于保存 Azure 解决方案相关资源的容器。 如果未指定现有资源组，该工具将创建一个。 | 可选 | [使用 Azure 门户管理 Azure 资源管理器资源组](/azure-resource-manager/manage-resource-groups-portal) |
| 私钥公钥对 | 若要使用从开发计算机到 Azure Stack Hub 实例中服务器 VM（用于托管 Web 应用）的开放 SSH 连接，需要创建安全外壳 (SSH) 公钥和私钥对。 | 必须 | 有关如何生成密钥的说明，请参阅 [SSH 密钥生成](/azure-stack/user/azure-stack-dev-start-howto-ssh-public-key)。|


> [!Note]  
> 还可以使用 [Azure Stack Hub 的 Azure CLI](/azure-stack/user/azure-stack-version-profiles-azurecli2) 或 [Azure Stack Hub PowerShell](/azure-stack/operator/azure-stack-powershell-install) 创建 AKS 引擎的先决条件。

## <a name="matching-engine-to-base-image-version"></a>将引擎与基础映像版本相匹配

AKS 引擎将自定义 Ubuntu Server OS 部署到每个群集节点映像，即“AKS Ubuntu 16.04-LTS 基础映像发行版”。 任何 AKS 引擎版本都依赖于 Azure Stack Hub 操作员在 Azure Stack Hub 中提供的特定映像版本。 可以在[支持的 Kubernetes 版本](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#supported-aks-engine-versions)中找到列出了 AKS 引擎版本和相应的受支持 Kubernetes 版本的表。 例如，AKS 引擎版本 `v0.51.0` 依赖于 AKS Ubuntu 16.04-LTS 基础映像分发版版本 `2020.05.13`。 请求 Azure Stack Hub 操作员将特定的映像版本从 Azure 市场下载到 Azure Stack Hub 市场。

如果映像在 Azure Stack Hub 市场中不可用，则会触发错误。 例如，如果当前使用的是 AKS 引擎 v0.51.0 版，并且 AKS Ubuntu 16.04-LTS 基础映像发行版 `2020.05.13` 不可用，则运行 AKS 引擎时会显示以下错误： 

```Text  
The platform image 'microsoft-aks:aks:aks-ubuntu-1604-202003:2020.05.13' is not available. 
Verify that all fields in the storage profile are correct.
```

可以运行以下命令查看 AKS 引擎的当前版本：

```bash  
$ aks-engine version
Version: v0.51.0
GitCommit: 44a35c00c
GitTreeState: clean
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [在 Azure Stack Hub 中的 Windows 上部署 AKS 引擎](azure-stack-kubernetes-aks-engine-deploy-windows.md)  
> [在 Azure Stack Hub 中的 Linux 上部署 AKS 引擎](azure-stack-kubernetes-aks-engine-deploy-linux.md)