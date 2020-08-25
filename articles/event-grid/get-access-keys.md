---
title: 获取事件网格资源的访问密钥
description: 本文介绍如何获取事件网格主题或域的访问密钥
ms.topic: how-to
author: Johnnytechn
ms.author: v-johya
ms.date: 08/10/2020
ms.openlocfilehash: ed1f3f7241a75b46d67536b02aec103c1cfc7194
ms.sourcegitcommit: 9d9795f8a5b50cd5ccc19d3a2773817836446912
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88228944"
---
# <a name="get-access-keys-for-event-grid-resources-topics-or-domains"></a>获取事件网格资源（主题或域）的访问密钥
访问密钥用于对将事件发布到 Azure 事件网格资源（主题和域）的应用程序进行身份验证。 我们建议定期重新生成密钥并安全地存储它们。 你拥有两个访问密钥，因此，在重新生成其中一个密钥时，可以使用另一个密钥来保持连接。

本文介绍如何使用 Azure 门户、PowerShell 或 CLI 获取事件网格资源（主题或域）的访问密钥。 

## <a name="azure-portal"></a>Azure 门户
在 Azure 门户中，切换到主题或域的“事件网格主题”或“事件网格域”页的“访问密钥”选项卡。  

:::image type="content" source="./media/get-access-keys/azure-portal.png" alt-text="“访问密钥”页":::

## <a name="azure-powershell"></a>Azure PowerShell
使用 [Get-AzEventGridTopicKey](https://docs.microsoft.com/powershell/module/az.eventgrid/get-azeventgridtopickey?view=azps-4.3.0) 命令获取主题的访问密钥。 

```azurepowershell
Get-AzEventGridTopicKey -ResourceGroup <RESOURCE GROUP NAME> -Name <TOPIC NAME>
```

使用 [Get-AzEventGridDomainKey](https://docs.microsoft.com/powershell/module/az.eventgrid/get-azeventgriddomainkey?view=azps-4.3.0) 命令获取域的访问密钥。 

```azurepowershell
Get-AzEventGridDomainKey -ResourceGroup <RESOURCE GROUP NAME> -Name <DOMAIN NAME>
```

## <a name="azure-cli"></a>Azure CLI
使用 [az eventgrid topic key list](https://docs.microsoft.com/cli/azure/ext/eventgrid/eventgrid/topic/key?view=azure-cli-latest) 获取主题的访问密钥。 

```azurecli
az eventgrid topic key list --resource-group <RESOURCE GROUP NAME> --name <TOPIC NAME>
```

使用 [az eventgrid domain key list](https://docs.microsoft.com/cli/azure/ext/eventgrid/eventgrid/domain/key?view=azure-cli-latest) 获取域的访问密钥。 

```azurecli
az eventgrid domain key list --resource-group <RESOURCE GROUP NAME> --name <DOMAIN NAME>
```

## <a name="next-steps"></a>后续步骤
请参阅以下文章：[对发布客户端进行身份验证](security-authenticate-publishing-clients.md)。 

