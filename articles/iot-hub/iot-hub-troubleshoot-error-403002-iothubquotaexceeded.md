---
title: 排查 Azure IoT 中心错误 403002 IoTHubQuotaExceeded
description: 了解如何修复错误 403002 IoTHubQuotaExceeded
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
origin.date: 01/30/2020
ms.date: 02/17/2020
ms.author: v-yiso
ms.openlocfilehash: 5c5b7e5609de34931fbe34ee8eafe2c398fe8775
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77068595"
---
# <a name="403002-iothubquotaexceeded"></a>403002 IoTHubQuotaExceeded

本文介绍 **403002 IoTHubQuotaExceeded** 错误的原因和解决方案。

## <a name="symptoms"></a>症状

对 IoT 中心的所有请求都失败，并出现错误 **403002 IoTHubQuotaExceeded**。 在 Azure 门户中，无法加载 IoT 中心设备列表。

## <a name="cause"></a>原因

已超过 IoT 中心的每日消息配额。 

## <a name="solution"></a>解决方案

[升级或增加 IoT 中心的单位数](iot-hub-upgrade.md)，或等待下一 UTC 日期，以刷新每日配额。

## <a name="next-steps"></a>后续步骤

* 若要了解如何将操作计入配额（如孪生查询和直接方法），请参阅[了解 IoT 中心定价](iot-hub-devguide-pricing.md#charges-per-operation)
* 若要针对每日配额使用情况来设置监视，请设置一个警报，其指标为“使用的消息总数”。  有关分步说明，请参阅[使用 IoT 中心设置指标和警报](tutorial-use-metrics-and-diags.md#set-up-metrics)