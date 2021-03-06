---
services: virtual-machines
title: 设置 PowerShell
authors: JoeDavies-MSFT
solutions: ''
manager: timlt
editor: tysonn
ms.service: virtual-machines
origin.date: 05/12/2015
ms.date: 06/21/2017
ms.openlocfilehash: d4116c3a80949a5390ac9d3c6112318f0108269a
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
ms.locfileid: "20227819"
---
## <a name="setting-up-powershell"></a>设置 PowerShell

在使用 Azure PowerShell 之前，请执行以下步骤。

### <a name="verify-powershell-versions"></a>验证 PowerShell 版本

在使用 Windows PowerShell 之前，必须安装 Windows PowerShell 3.0 或 4.0 版。 若要查找 Windows PowerShell 版本，请在 Windows PowerShell 命令提示符下键入以下命令。

```
$PSVersionTable
```

你应看到类似如下的内容。

```
Name                           Value
----                           -----
PSVersion                      3.0
WSManStackVersion              3.0
SerializationVersion           1.1.0.1
CLRVersion                     4.0.30319.18444
BuildVersion                   6.2.9200.16481
PSCompatibleVersions           {1.0, 2.0, 3.0}
PSRemotingProtocolVersion      2.2
```

确保 **PSVersion** 的值为 3.0 或 4.0。 若要安装兼容版本，请参阅 [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) 或 [Windows Management Framework 4.0](http://www.microsoft.com/zh-CN/download/details.aspx?id=40855)。

还应安装 Azure PowerShell 0.8.0 或更高版本。 可以使用此命令在 Azure PowerShell 命令提示符下查看已安装的 Azure PowerShell 版本。

```
Get-Module azure | format-table version
```

你应看到类似如下的内容。

```
Version
-------
0.8.16.1
```

有关说明以及指向最新版本的链接，请参阅[如何安装和配置 Azure PowerShell](../articles/powershell-install-configure.md)。

### <a name="set-your-azure-account-and-subscription"></a>设置 Azure 帐户和订阅

如果还没有 Azure 订阅，可以注册一个[试用版](https://www.azure.cn/pricing/1rmb-trial/)。

打开 Azure PowerShell 命令提示符，然后使用此命令登录到 Azure。

```
Add-AzureAccount
```

如果有多个 Azure 订阅，可使用以下命令列出 Azure 订阅。

```
Get-AzureSubscription
```

你将收到以下类型的信息：

```
SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
SubscriptionName          : Visual Studio Ultimate with MSDN
Environment               : AzureCloud
SupportedModes            : AzureServiceManagement,AzureResourceManager
DefaultAccount            : johndoe@contoso.com
Accounts                  : {johndoe@contoso.com}
IsDefault                 : True
IsCurrent                 : True
CurrentStorageAccountName : 
TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7
```

可通过在 Azure PowerShell 命令提示符下运行以下命令设置当前的 Azure 订阅。 将引号内的所有内容（包括 < 和 > 字符）替换为相应的名称。

```
$subscr="<SubscriptionName from the display of Get-AzureSubscription>"
Select-AzureSubscription -SubscriptionName $subscr -Current 
```

有关 Azure 订阅和帐户的详细信息，请参阅[如何：连接到订阅](../articles/powershell-install-configure.md#Connect)。