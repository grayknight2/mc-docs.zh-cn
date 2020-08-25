---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 08/10/2020
ms.date: 08/24/2020
ms.author: v-jay
ms.openlocfilehash: e2ae94c2fdde963846c10735d31076e23ebeda75
ms.sourcegitcommit: ecd6bf9cfec695c4e8d47befade8c462b1917cf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2020
ms.locfileid: "88753306"
---
#### <a name="additional-premium-file-share-level-limits"></a>其他高级文件共享级别限制

|区域  |目标  |
|---------|---------|
|最小大小增加/减少    |1 GiB      |
|基线 IOPS    |每个 GiB 1 IOPS，最高 100,000|
|IOPS 突发    |每个 GiB 3倍 IOPS，最高 100,000|
|出口速率         |60 MiB/秒 + 0.06 * 预配 GiB        |
|入口速率| 40 MiB/秒 + 0.04 * 预配 GiB |

#### <a name="file-level-limits"></a>文件级别限制

|区域  |标准文件  |高级文件  |
|---------|---------|---------|
|大小     |1 TiB         |4 TiB         |
|每个文件的最大 IOPS      |1,000         |5,000         |
|并发句柄数     |2,000         |2,000         |
|流出量     |查看标准文件吞吐量值         |300 MiB/秒         |
|流入量     |查看标准文件吞吐量值         |200 MiB/秒         |
|吞吐量     |最多 60 MiB/秒         |查看高级文件流入量/流出量值         |