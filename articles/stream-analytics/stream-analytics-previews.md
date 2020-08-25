---
title: Azure 流分析预览功能
description: 本文列出了当前以预览版提供的 Azure 流分析功能。
author: Johnnytechn
ms.author: v-johya
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/20/2020
ms.openlocfilehash: 214177fe7910fcd9e27ccce4b9ad1799be6645fd
ms.sourcegitcommit: 09c7071f4d0d9256b40a6bf700b38c6a25db1b26
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2020
ms.locfileid: "88715776"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure 流分析预览功能

本文汇总了当前以预览版提供的 Azure 流分析的所有功能。 建议不要在生产环境中使用预览功能。

## <a name="public-previews"></a>公共预览版

以下功能以公共预览版提供。 现在可以使用这些功能，但请勿在生产环境中使用它们。

### <a name="online-scaling"></a>联机缩放

通过联机缩放，如果需要更改 SU 分配，无需停止作业。 可以增加或减少正在运行的作业的 SU 容量，而无需将其停止。 这基于客户对流分析目前提供的长时间运行的任务关键管道的承诺。 有关详细信息，请参阅[配置 Azure 流分析流式处理单元](stream-analytics-streaming-unit-consumption.md#configure-stream-analytics-streaming-units-sus)。

### <a name="real-time-high-performance-scoring-with-custom-ml-models-managed-by-azure-machine-learning"></a>通过 Azure 机器学习管理的自定义 ML 模型进行实时高性能评分

Azure 流分析通过利用自定义预先训练的机器学习模型（由 Azure 机器学习管理，并在 Azure Kubernetes 服务 (AKS) 或 Azure 容器实例 (ACI) 中托管），并使用不需要编写代码的工作流，来支持高性能实时评分。 [注册](https://aka.ms/asapreview1)即可获取预览版

<!--.NET user-defined-function is not available in China.-->
## <a name="other-previews"></a>其他预览

如若请求，还可以使用以下预览功能。

### <a name="support-for-azure-stack"></a>支持 Azure Stack
此功能在 Azure IoT Edge 运行时启用，利用自定义 Azure Stack 功能，比如以原生方式支持在 Azure Stack 上运行的本地输入和输出（例如事件中心、IoT 中心、Blob 存储）。 利用这一新的集成，你可以构建混合体系结构，该体系结构可以在数据生成的位置附近分析数据，从而降低延迟并最大程度增加见解。
可以通过此功能构建混合体系结构，该体系结构可以在数据生成的位置附近分析数据，从而降低延迟并最大程度增加见解。 必须[注册](https://aka.ms/asapreview1)才能获取此预览版。
