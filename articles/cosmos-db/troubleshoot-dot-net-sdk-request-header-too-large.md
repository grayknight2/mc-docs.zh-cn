---
title: 排查 Azure Cosmos DB 中的“请求标头太大”或“400 错误请求”问题
description: 如何诊断和修复“请求标头太大”异常
author: rockboyfor
ms.service: cosmos-db
origin.date: 07/13/2020
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: d88dcb74a6f8be6334d52d4bd1de6188e6d9203b
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223595"
---
<!--Verified successfully-->
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-request-header-too-large-message"></a>诊断并排查出现 Azure Cosmos DB 消息“请求标头太大”时的问题
引发了“请求标头太大”消息，HTTP 错误代码为 400。 如果请求标头大小变得很大，即将超过允许的最大大小，则会发生此错误。 建议使用 SDK 的最新版本。 请确保至少使用版本 3.x 或 2.x，因为这些版本会在异常消息中添加标头大小跟踪。

## <a name="troubleshooting-steps"></a>疑难解答步骤
如果会话或继续标记太大，则会出现“请求标头太大”消息。 以下各部分介绍了每个类别中该问题的原因和解决方案。

### <a name="1-session-token-too-large"></a>1.会话标记太大

#### <a name="cause"></a>原因：
400 请求错误很可能是由于会话标记太大导致的。 如果以下陈述属实，则表明会话标记确实太大。

* 此错误发生在其中没有继续标记的点操作（例如创建、读取、更新等）上。
* 异常启动时没有对应用程序进行任何更改。 会话标记随着容器中分区数量的增加而增大。 随着数据量增加或吞吐量增加，分区数量也增加。

#### <a name="temporary-mitigation"></a>临时缓解： 
重启你的客户端应用程序以重置所有会话标记。 会话标记最终会恢复先前的导致该问题的大小。 因此，请使用下一部分中的解决方案来完全避免此问题。

#### <a name="solution"></a>解决方案：
1. 按照 [.NET V3](performance-tips-dotnet-sdk-v3-sql.md) 或 [.NET V2](performance-tips.md) 性能提示一文中的指南操作，然后对应用程序进行转换，以便使用采用 TCP 协议的直接连接模式。 采用 TCP 协议的直接模式对标头大小没有限制（HTTP 协议则具有限制），因此可避免此问题。 请确保使用最新版本的 SDK，此版本针对不可使用服务互操作时进行的查询操作提供了修复。
2. 如果采用 TCP 协议的直接连接模式不是适用于你的工作负载的选项，请通过更改[客户端一致性级别](how-to-manage-consistency.md)来缓解此情况。 会话标记仅用于实现会话一致性（这是 Azure Cosmos DB 的默认一致性级别）。 其他一致性级别不使用会话标记。

### <a name="2-continuation-token-too-large"></a>2.继续标记太大

#### <a name="cause"></a>原因：
400 错误请求发生在使用了继续标记的查询操作中。 如果继续标记变得太大或不同的查询具有不同的继续标记大小，则会发生此错误。

#### <a name="solution"></a>解决方案：
1. 按照 [.NET V3](performance-tips-dotnet-sdk-v3-sql.md) 或 [.NET V2](performance-tips.md) 性能提示一文中的指南操作，然后对应用程序进行转换，以便使用采用 TCP 协议的直接连接模式。 采用 TCP 协议的直接模式对标头大小没有限制（HTTP 协议则具有限制），因此可避免此问题。 
3. 如果采用 TCP 协议的直接连接模式不是适用于你的工作负载的选项，则尝试设置 `ResponseContinuationTokenLimitInKb` 选项。 对于 v2，可以在 `FeedOptions` 中找到此选项；对于 v3，可以在 `QueryRequestOptions` 中找到此选项。

## <a name="next-steps"></a>后续步骤
* [诊断和排查](troubleshoot-dot-net-sdk.md)使用 Azure Cosmos DB .NET SDK 时遇到的问题
* 了解 [.NET V3](performance-tips-dotnet-sdk-v3-sql.md) 和 [.NET V2](performance-tips.md) 的性能准则

<!-- Update_Description: new article about troubleshoot dot net sdk request header too large -->
<!--NEW.date: 08/10/2020-->