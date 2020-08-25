---
title: 排查 Azure Cosmos DB 未经授权异常
description: 如何诊断和修复未经授权异常
author: rockboyfor
ms.service: cosmos-db
origin.date: 07/13/2020
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 062bf1bff97e3a632c534bdda91216278973df0f
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223590"
---
<!--Verified successfully-->
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-unauthorized-exception"></a>诊断和排查 Azure Cosmos DB 未经授权异常

HTTP 401：HTTP 请求中的 MAC 签名与计算出的签名不同。
如果收到以下 401 错误消息：“HTTP 请求中的 MAC 签名与计算出的签名不同。” 此错误可能是由以下情况造成的。

在较旧的 SDK 中，异常可能显示为无效的 json 异常，而不是正确的 401 未经授权异常。 较新的 SDK 可以正确处理此情况，并提供有效的错误消息。

## <a name="troubleshooting-steps"></a>疑难解答步骤
以下列表包含未经授权异常的已知原因和解决方案。

### <a name="1-key-was-not-properly-rotated-is-the-most-common-scenario"></a>1.密钥未正确轮换是最常见的情况。
密钥轮换之后不久就会出现 401 MAC 签名问题，但无需进行任何更改，它最终会停止。 

#### <a name="solution"></a>解决方案：
密钥已轮换，且未遵循[最佳做法](secure-access-to-data.md#key-rotation)。 完成 Cosmos DB 帐户密钥轮换耗时几秒到几天不等，具体取决于 Cosmos DB 帐户大小。

### <a name="2-the-key-is-misconfigured"></a>2.密钥配置错误 
401 MAC 签名问题持续出现并在使用该密钥的所有调用中发生

#### <a name="solution"></a>解决方案：
密钥在应用程序上配置错误，并且帐户使用了错误密钥或未复制整个密钥。

### <a name="3-the-application-is-using-the-read-only-keys-for-write-operations"></a>3.应用程序使用只读密钥进行写入操作
401 MAC 签名问题仅在创建或替换等写入操作中发生，但读取请求会成功。

#### <a name="solution"></a>解决方案：
切换应用程序以使用读/写密钥，从而允许操作成功完成。

### <a name="4-race-condition-with-create-container"></a>4.创建容器时出现争用情况
创建容器后不久看到 401 MAC 签名问题。 此问题仅在容器创建完成后发生。

#### <a name="solution"></a>解决方案：
创建容器时出现争用状况。 在完成容器创建之前，某个应用程序实例正在尝试访问容器。 出现此争用条件的最常见情况是，应用程序正在运行就删除了容器，并重新创建了同名的容器。 SDK 将尝试使用新容器，但由于容器创建仍在进行，因此无法获得密钥。

## <a name="next-steps"></a>后续步骤
* [诊断和排查](troubleshoot-dot-net-sdk.md)使用 Azure Cosmos DB .NET SDK 时遇到的问题
* 了解 [.NET V3](performance-tips-dotnet-sdk-v3-sql.md) 和 [.NET V2](performance-tips.md) 的性能准则

<!-- Update_Description: new article about troubleshoot unauthorized -->
<!--NEW.date: 08/10/2020-->