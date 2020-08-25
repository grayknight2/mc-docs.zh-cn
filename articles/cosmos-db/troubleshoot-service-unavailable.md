---
title: 排查“Azure Cosmos DB 服务不可用”异常
description: 如何诊断和修复“Cosmos DB 服务不可用”异常
author: rockboyfor
ms.service: cosmos-db
origin.date: 07/13/2020
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: c9dcaf03f45826de0d90301875c9d6cafe9b6505
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223591"
---
<!--Verified successfully-->
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-service-unavailable"></a>诊断和排查“Azure Cosmos DB 服务不可用”问题
SDK 无法连接到 Azure Cosmos DB 服务。

## <a name="troubleshooting-steps"></a>疑难解答步骤
下面的列表包含“服务不可用”异常的已知原因和解决方案。

### <a name="1-the-required-ports-are-not-enabled"></a>1.未启用所需的端口。
验证所有[必需的端口](performance-tips-dotnet-sdk-v3-sql.md#networking)是否已启用。

## <a name="if-an-existing-application-or-service-started-getting-503"></a>如果现有的应用程序或服务已开始收到 503

### <a name="1-there-is-an-outage"></a>1.发生中断
检查 [Azure 状态](https://status.azure.com/status)，看是否正在发生问题。

### <a name="2-snat-port-exhaustion"></a>2.SNAT 端口耗尽
按照 [SNAT 端口耗尽指南](troubleshoot-dot-net-sdk.md#snat)操作。

### <a name="3-the-required-ports-are-being-blocked"></a>3.所需端口被阻止
验证所有[必需的端口](performance-tips-dotnet-sdk-v3-sql.md#networking)是否已启用。

## <a name="next-steps"></a>后续步骤
* [诊断和排查](troubleshoot-dot-net-sdk.md)使用 Azure Cosmos DB .NET SDK 时遇到的问题
* 了解 [.NET V3](performance-tips-dotnet-sdk-v3-sql.md) 和 [.NET V2](performance-tips.md) 的性能准则

<!-- Update_Description: new article about troubleshoot service unavailable -->
<!--NEW.date: 08/10/2020-->