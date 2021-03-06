---
title: Azure HDInsight 群集中出现监视器 BUG 软锁定 CPU 错误
description: 内核 syslog 中出现监视器 BUG 软锁定 CPU
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
origin.date: 08/05/2019
ms.date: 09/23/2019
ms.openlocfilehash: 73c3b3b17c52ae069d00aeefe95c6f098919fa9e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "70921335"
---
# <a name="scenario-watchdog-bug-soft-lockup---cpu-error-from-an-azure-hdinsight-cluster"></a>场景：Azure HDInsight 群集中出现“监视器:BUG: 软锁定 - CPU”错误

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方法。

## <a name="issue"></a>问题

内核 syslog 包含错误消息：`watchdog: BUG: soft lockup - CPU`。

## <a name="cause"></a>原因

Linux 内核中的一个 [bug](https://bugzilla.kernel.org/show_bug.cgi?id=199437) 导致 CPU 软锁定。

## <a name="resolution"></a>解决方法

应用内核修补程序。 以下脚本将升级 Linux 内核，并在 24 小时内的不同时间重新启动计算机。 在两个批处理文件中执行脚本操作。 第一个批处理文件在除头节点以外的所有节点上运行。 第二个批处理文件在头节点上运行。 不要同时在头节点和其他节点上运行。

1. 从 Azure 门户导航到你的 HDInsight 群集。

1. 转到“脚本操作”。

1. 选择“提交新项”并按如下所示提供输入 

    | 属性 | Value |
    | --- | --- |
    | 脚本类型 | -Custom |
    | 名称 |内核软锁定问题的修复 |
    | Bash 脚本 URI |`https://raw.githubusercontent.com/hdinsight/hdinsight.github.io/master/ClusterCRUD/KernelSoftLockFix/scripts/KernelSoftLockIssue_FixAndReboot.sh` |
    | 节点类型 |Worker、Zookeeper |
    | parameters |空值 |

    若要在添加新节点时执行脚本，请选择“保留此脚本操作...”。 

1. 选择“创建”  。

1. 等待执行成功。

1. 按照与步骤 3 相同的步骤在头节点上执行脚本操作，但这一次需使用节点类型：Head。

1. 等待执行成功。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：


* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 在 Microsoft Azure 订阅中可以访问订阅管理和计费支持；通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
