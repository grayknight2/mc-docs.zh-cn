---
title: Azure 事件中心的安全控制
description: 本文提供了用于评估 Azure 事件中心（网络、标识、数据保护等）的安全控制清单。
ms.topic: conceptual
origin.date: 06/23/2020
ms.date: 08/21/2020
ms.author: v-tawe
ms.openlocfilehash: 751de9fd6bb06cbfa9d1e4168627ff85444ef02b
ms.sourcegitcommit: 2e9b16f155455cd5f0641234cfcb304a568765a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2020
ms.locfileid: "88715205"
---
# <a name="security-controls-for-azure-event-hubs"></a>Azure 事件中心的安全控制

本文介绍 Azure 事件中心中内置的安全控制。

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>网络

| 安全控制 | Yes/No | 注释 | 文档 |
|---|---|--|--|
| 服务终结点支持| 是 |  |  |
| VNet 注入支持| 否 | |  |
| 网络隔离和防火墙支持| 是 |  |  |
| 强制隧道支持| 否 |  |  |

## <a name="monitoring--logging"></a>监视和日志记录

| 安全控制 | Yes/No | 注释| 文档 |
|---|---|--|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 是 | |  |
| 控制和管理平面日志记录和审核| 是 |  |  |
| 数据平面日志记录和审核| 是 |   |  |

## <a name="identity"></a>标识

| 安全控制 | Yes/No | 注释| 文档 |
|---|---|--|--|
| 身份验证| 是 | | [授权访问 Azure 事件中心](authorize-access-event-hubs.md)、[使用 Azure Active Directory 授权访问事件中心资源](authorize-access-azure-active-directory.md)、[使用共享访问签名授权访问事件中心资源](authorize-access-shared-access-signature.md) |
| 授权|  是 | | [使用 Azure Active Directory 验证托管标识以访问事件中心资源](authenticate-managed-identity.md)、[使用 Azure Active Directory 验证应用程序以访问事件中心资源](authenticate-application.md)、[使用共享访问签名 (SAS) 验证对事件中心资源的访问](authenticate-shared-access-signature.md) |

## <a name="data-protection"></a>数据保护

<!-- BYOK not supported -->

| 安全控制 | Yes/No | 注释 | 文档 |
|---|---|--|--|
| 服务器端静态加密：Microsoft 管理的密钥 |  是 | |  |
| 列级加密（Azure 数据服务）| 空值 | |  |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 是 | |  |
| 加密的 API 调用| 是 |  |  |

## <a name="configuration-management"></a>配置管理

| 安全控制 | Yes/No | 注释| 文档 |
|---|---|--|--|
| 配置管理支持（配置的版本控制等）| 是 | |  |

## <a name="next-steps"></a>后续步骤

- 详细了解[跨 Azure 服务的内置安全控制](../security/fundamentals/security-controls.md)。
