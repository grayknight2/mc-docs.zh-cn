---
title: Windows 在启动 Azure VM 时显示“正在检查文件系统”
description: 了解如何解决 VM 在启动时显示“正在检查文件系统”的问题 | Azure
services: virtual-machines-windows
documentationCenter: ''
author: rockboyfor
manager: digimobile
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 08/31/2018
ms.date: 04/27/2020
ms.author: v-yeche
ms.openlocfilehash: 3922ffb93f6d964c314484c62efd81dadf84441d
ms.sourcegitcommit: b469d275694fb86bbe37a21227e24019043b9e88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82596036"
---
# <a name="windows-shows-checking-file-system-when-booting-an-azure-vm"></a>Windows 在启动 Azure VM 时显示“正在检查文件系统”

本文介绍在 Azure 中启动 Windows 虚拟机 (VM) 时可能遇到的“正在检查文件系统”错误。

## <a name="symptom"></a>症状 

Windows VM 不启动。 检查[启动诊断](boot-diagnostics.md)中的启动屏幕截图时，会看到“磁盘检查”进程 (chkdsk.exe) 正在运行，同时出现以下一条消息：

- 扫描并修复驱动器 (C:)
- 检查 C: 盘上的文件系统

## <a name="cause"></a>原因

如果文件系统中出现 NTFS 错误，Windows 会在下次重启时检查并修复磁盘的一致性。 通常，如果 VM 发生意外重启或 VM 关闭过程突然中断，则会出现此情况。

## <a name="solution"></a>解决方案 

“磁盘检查”进程完成后，Windows 会正常启动。 如果 VM 一直卡在“磁盘检查”进程中，请尝试脱机后在 VM 上运行“磁盘检查”：
1. 拍摄受影响的 VM 的 OS 磁盘的快照作为备份。 有关详细信息，请参阅[拍摄磁盘快照](../windows/snapshot-copy-managed-disk.md)。
2. [将 OS 磁盘附加到恢复 VM](troubleshoot-recovery-disks-portal-windows.md)。  
3. 在恢复 VM 上，在附加的 OS 磁盘上运行“磁盘检查”。 在以下示例中，附加的 OS 磁盘的驱动器号为 E: 

        chkdsk E: /f
4. “磁盘检查”完成后，从恢复 VM 分离该磁盘，然后将磁盘作为 OS 磁盘重新附加到受影响的 VM。 有关详细信息，请参阅[通过将 OS 磁盘附加到恢复 VM 来排查 Windows VM 相关问题](troubleshoot-recovery-disks-portal-windows.md)。

<!-- Update_Description: update meta properties, wording update, update link -->