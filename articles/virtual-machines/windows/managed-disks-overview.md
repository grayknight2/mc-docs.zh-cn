---
title: Azure 托管磁盘简介
description: 概述了 Azure 托管磁盘，这种磁盘在你使用 Azure VM 时为你处理存储帐户
author: rockboyfor
ms.service: virtual-machines
ms.topic: conceptual
origin.date: 04/24/2020
ms.date: 08/10/2020
ms.testscope: no
ms.testdate: 08/24/2020
ms.author: v-yeche
ms.subservice: disks
ms.openlocfilehash: c3acbfc406408dcbb9f98176bfe7d17b1647c01f
ms.sourcegitcommit: c4b0a0ddd52ceca82cfdbdbdf07aad00d0f3ed03
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2020
ms.locfileid: "88714507"
---
# <a name="introduction-to-azure-managed-disks"></a>Azure 托管磁盘简介

Azure 托管磁盘是由 Azure 托管并与 Azure 虚拟机配合使用的块级存储卷。 托管磁盘类似于本地服务器中的物理磁盘，但它是虚拟化的。 使用托管磁盘时，只需指定磁盘大小、磁盘类型并预配磁盘即可。 预配此磁盘后，Azure 会处理剩余磁盘。

可用的磁盘类型包括高级固态硬盘 (SSD)、标准 SSD 和标准硬盘驱动器 (HDD)。 有关每种磁盘类型的信息，请参阅[选择适用于 IaaS VM 的磁盘类型](disks-types.md)。

<!--Not Available on ultra disks-->

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [选择适用于 IaaS VM 的磁盘类型](disks-types.md)

<!-- Update_Description: update meta properties, wording update -->