---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 11/09/2018
ms.date: 08/10/2020
ms.testscope: no
ms.testdate: 05/20/2019
ms.author: v-yeche
ms.openlocfilehash: 04d33865beab361f2bda186e3d962ac5dc243c3c
ms.sourcegitcommit: ac70b12de243a9949bf86b81b2576e595e55b2a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "88753617"
---
| 资源 | 限制 |
| --- | --- |
| 每个云服务的虚拟机数 <sup>1</sup> |50 |
| 每个云服务的输入终结点数 <sup>2</sup> |150 |

<sup>1</sup> 通过使用经典部署模型（而不是 Azure 资源管理器）创建的虚拟机自动存储在云服务中。 可以向该云服务添加更多虚拟机以获得负载均衡和可用性。 

<sup>2</sup> 输入终结点允许从某个虚拟机的云服务外部与该虚拟机通信。 位于同一云服务或虚拟网络中的虚拟机可以自动相互通信。

<!-- Update_Description: update meta properties, wording update, update link -->