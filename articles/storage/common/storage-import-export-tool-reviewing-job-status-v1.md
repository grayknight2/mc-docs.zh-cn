---
title: 查看 Azure 导入/导出作业状态 - v1 | Microsoft Docs
description: 了解如何使用运行导入或导出作业时创建的日志文件查看导入/导出作业的状态。
author: WenJason
services: storage
ms.service: storage
ms.topic: article
origin.date: 01/26/2017
ms.date: 03/09/2020
ms.author: v-jay
ms.subservice: common
ms.openlocfilehash: 319704dd59ab4bae5bcfa8286a08b52dbc36ffaf
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78411773"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>使用复制日志文件查看 Azure 导入/导出作业状态
Azure 导入/导出服务在处理与某个导入或导出作业关联的驱动器时，会将日志文件写入到要在其中导入或导出 Blob 的存储帐户。 该日志文件包含与已导入或导出的每个文件相关的详细状态。 查询某个已完成作业的状态时，将返回每个复制日志文件的 URL；有关详细信息，请参阅[获取作业](https://docs.microsoft.com/rest/api/storageimportexport/Jobs/Get)。  

## <a name="example-urls"></a>示例 URL

下面是包含两个驱动器的导入作业的复制日志文件的示例 URL：  

 `http://myaccount.blob.core.chinacloudapi.cn/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  

 `http://myaccount.blob.core.chinacloudapi.cn/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  

 有关复制日志的格式以及状态代码的完整列表，请参阅[导入/导出服务日志文件格式](../storage-import-export-file-format-log.md)。  

## <a name="next-steps"></a>后续步骤

 * [设置 Azure 导入/导出工具](storage-import-export-tool-setup-v1.md)   
 * [为导入作业准备硬盘驱动器](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [修复导入作业](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [修复导出作业](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [排查 Azure 导入/导出工具问题](storage-import-export-tool-troubleshooting-v1.md)
