---
author: rockboyfor
ms.service: virtual-machines-linux
ms.topic: include
origin.date: 11/25/2018
ms.date: 12/24/2018
ms.author: v-yeche
ms.openlocfilehash: 231158a80ba7cb0f981c2376591ec625691a9334
ms.sourcegitcommit: 96ceb27357f624536228af537b482df08c722a72
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/21/2018
ms.locfileid: "53736228"
---
有关磁盘的详细信息，请参阅[关于虚拟机的磁盘和 VHD](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)。

<a name="attachempty"></a>

## <a name="attach-an-empty-disk"></a>附加空磁盘
1. 打开 Azure 经典 CLI 并[连接到 Azure 订阅](https://docs.azure.cn/zh-cn/cli/authenticate-azure-cli?view=azure-cli-latest)。 确保是在 Azure 服务管理模式 (`azure config mode asm`) 下。
2. 输入 `azure vm disk attach-new` 以创建并附加新磁盘，如以下示例所示。 将 *myVM* 替换为 Linux 虚拟机的名称，并指定磁盘的大小（以 GB 为单位），在此示例中为 *100 GB*：

    ```azurecli
    azure vm disk attach-new myVM 100
    ```

3. 创建并附加数据磁盘后，它列出在 `azure vm disk list <virtual-machine-name>` 的输出中，如以下示例所示：

    ```azurecli
    azure vm disk list TestVM
    ```

    输出类似于以下示例：

    ```bash
    info:    Executing command vm disk list

    * Fetching disk images
    * Getting virtual machines
    * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        myVM-2645b8030676c8f8.vhd  Linux
     data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

<a name="attachexisting"></a>

## <a name="attach-an-existing-disk"></a>附加现有磁盘
附加现有磁盘需要存储帐户中具有可用的 .vhd。

1. 打开 Azure 经典 CLI 并[连接到 Azure 订阅](https://docs.azure.cn/zh-cn/cli/authenticate-azure-cli?view=azure-cli-latest)。 确保是在 Azure 服务管理模式 (`azure config mode asm`) 下。
2. 检查要附加的 VHD 是否已上传到 Azure 订阅：

    ```azurecli
    azure vm disk list
    ```

    输出类似于以下示例：

    ```azurecli
     info:    Executing command vm disk list

    * Fetching disk images
     data:    Name                                          OS
     data:    --------------------------------------------  -----
     data:    myTestVhd                                     Linux
     data:    TestVM-ubuntuVMasm-0-201508060029150744  Linux
     data:    TestVM-ubuntuVMasm-0-201508060040530369
     info:    vm disk list command OK
    ```

3. 如果未找到要使用的磁盘，可以使用 `azure vm disk create` 或 `azure vm disk upload` 将本地 VHD 上传到订阅。 以下是 `disk create` 的一个示例：

    ```azurecli
    azure vm disk create myVhd .\TempDisk\test.VHD -l "China East" -o Linux
    ```

    输出类似于以下示例：

    ```azurecli
    info:    Executing command vm disk create
    + Retrieving storage accounts
    info:    VHD size : 10 GB
    info:    Uploading 10485760.5 KB
    Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
    info:    Finishing computing MD5 hash, 16% is complete.
    info:    https://mystorageaccount.blob.core.chinacloudapi.cn/disks/myVHD.vhd was
    uploaded successfully
    info:    vm disk create command OK
    ```

   也可以使用 `azure vm disk upload` 将 VHD 上传到特定存储帐户中。 详细阅读命令，以管理[这里](https://docs.azure.cn/zh-cn/cli/get-started-with-az-cli2?view=azure-cli-latest)的 Azure 虚拟机数据磁盘。

4. 现在将所需 VHD 附加到虚拟机：

    ```azurecli
    azure vm disk attach myVM myVhd
    ```

   请确保将 *myVM* 替换为虚拟机的名称，将 *myVHD* 替换为所需的 VHD。

5. 可以使用 `azure vm disk list <virtual-machine-name>`验证磁盘是否已附加到虚拟机：

    ```azurecli
    azure vm disk list myVM
    ```

    输出类似于以下示例：

    ```azurecli
     info:    Executing command vm disk list

    * Fetching disk images
    * Getting virtual machines
    * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        TestVM-2645b8030676c8f8.vhd  Linux
     data:    1    10        test.VHD
     data:    0    100        TestVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

> [!NOTE]
> 在添加数据磁盘后，需要登录到虚拟机并初始化磁盘，然后虚拟机才能使用磁盘来存储数据（有关如何初始化磁盘的详细信息，请参阅以下步骤）。
> 
>

<!-- Update_Description: update meta properties -->