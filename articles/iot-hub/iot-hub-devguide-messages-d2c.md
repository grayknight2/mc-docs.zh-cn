---
title: 了解 Azure IoT 中心消息路由 | Microsoft Docs
description: 开发人员指南 - 如何使用消息路由发送设备到云的消息。 包含有关发送遥测和非遥测数据的信息。
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
origin.date: 05/15/2019
ms.date: 04/06/2020
ms.author: v-yiso
ms.openlocfilehash: 4224b503a535e8894cc7fc3edd8f1b866104b6e1
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86225986"
---
# <a name="use-iot-hub-message-routing-to-send-device-to-cloud-messages-to-different-endpoints"></a>使用 IoT 中心消息路由将设备到云消息发送到不同的终结点

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

消息路由使你能够以自动、可缩放以及可靠的方式将消息从设备发送到云服务。 消息路由可用于： 

* **发送设备遥测消息以及事件**（即设备生命周期事件和设备孪生更改事件）到内置终结点和自定义终结点。 了解有关[路由终结点](#routing-endpoints)。

* **在将数据路由到各个终结点之前对数据进行筛选**，筛选方法是通过应用丰富的查询。 消息路由允许你查询消息属性和消息正文以及设备孪生标记和设备孪生属性。 深入了解如何使用[消息路由中的查询](iot-hub-devguide-routing-query-syntax.md)。

IoT 中心需要这些服务终结点的写入权限，以便使用消息路由。 如果通过 Azure 门户配置终结点，则为你添加必要权限。 请确保将服务配置为支持预期吞吐量。 例如，如果使用事件中心作为自定义终结点，则必须为该事件中心配置**吞吐量单位**，以便它可以处理你计划通过 IoT 中心消息路由发送的事件流入量。 同样，使用服务总线队列作为终结点时，必须配置**最大大小**，以确保队列可以容纳所有流入的数据，直到它被使用者传出。 在首次配置 IoT 解决方案时，可能需要监视附加终结点，并针对实际负载进行任意的必要调整。

IoT 中心为所有设备到云的消息传送定义了[格式](iot-hub-devguide-messages-construct.md)，以便实现跨协议互操作性。 如果某条消息与多个路由匹配，而这些路由指向同一终结点，则 IoT 中心仅向该终结点传递一次消息。 因此无需在服务总线队列或主题中配置重复数据删除。 在分区队列中，分区相关性可保障消息排序。 使用本教程了解如何[配置消息路由](tutorial-routing.md)。

## <a name="routing-endpoints"></a>路由终结点

IoT 中心有一个默认的内置终结点（消息/事件），此终结点与事件中心兼容。 可以通过将订阅中的其他服务链接到中心来创建要将消息路由到的[自定义终结点](iot-hub-devguide-endpoints.md#custom-endpoints)。 

每条消息都路由到与它的路由查询匹配的所有终结点。 换句话说，消息可以路由到多个终结点。




IoT 中心目前支持以下终结点：

 - 内置终结点
 - Azure 存储
 - 服务总线队列和服务总线主题
 - 事件中心

### <a name="built-in-endpoint"></a>内置终结点

可以使用标准[事件中心集成和 SDK](iot-hub-devguide-messages-read-builtin.md) 接收来自内置终结点（消息/事件）的设备到云的消息。 在创建一个路由后，数据将停止流向内置终结点，除非创建了到该终结点的路由。

### <a name="azure-storage"></a>Azure 存储

IoT 中心可将消息路由到以下两个存储服务：[Azure Blob 存储](../storage/blobs/storage-blobs-introduction.md)和 [Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md) (ADLS Gen2) 帐户。 Azure Data Lake Storage 帐户是在 Blob 存储之上构建的启用[分层命名空间](../storage/blobs/data-lake-storage-namespace.md)的存储帐户。 这两个存储服务都使用 blob 作为其存储。

IoT 中心支持将数据以 [Apache Avro](https://avro.apache.org/) 格式和 JSON 格式写入 Azure 存储。 默认值为 AVRO。 只有在配置 Blob 存储终结点时才能设置编码格式。 不能编辑现有终结点的格式。 使用 JSON 编码时，必须在消息[系统属性](iot-hub-devguide-routing-query-syntax.md#system-properties)中将 contentType 设置为 **application/json**，将 contentEncoding 设置为 **UTF-8**。 这两个值都不区分大小写。 如果未设置内容编码，则 IoT 中心将以 base 64 编码格式写入消息。 可以使用 IoT 中心的创建或更新 REST API（具体说来就是 [RoutingStorageContainerProperties](https://docs.microsoft.com/rest/api/iothub/iothubresource/createorupdate#routingstoragecontainerproperties)、Azure 门户、[Azure CLI](https://docs.microsoft.com/cli/azure/iot/hub/routing-endpoint?view=azure-cli-latest) 或 [Azure Powershell](https://docs.microsoft.com/powershell/module/az.iothub/add-aziothubroutingendpoint?view=azps-1.3.0)）选择编码格式。 下图说明如何在 Azure 门户中选择编码格式。

![Blob 存储终结点编码](./media/iot-hub-devguide-messages-d2c/blobencoding.png)

IoT 中心将在消息达到特定大小或在经过一定的时间后，对消息进行批处理并将数据写入存储。 IoT 中心默认为以下文件命名约定： 

```
{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}
```

你可以使用任何文件命名约定，但必须使用所有列出的令牌。 如果没有要写入的数据，IoT 中心会写入到一个空 blob。

我们建议列出 blob 或文件，然后循环访问它们，以确保在未进行有关分区的任何假设的情况下读取所有 blob 或文件。 在 [Microsoft 发起的故障转移](iot-hub-ha-dr.md#microsoft-initiated-failover)或 IoT 中心[手动故障转移](iot-hub-ha-dr.md#manual-failover)期间，分区范围可能发生变化。 可以使用 [List Blobs API](https://docs.microsoft.com/rest/api/storageservices/list-blobs) 枚举 blob 列表，或使用 [List ADLS Gen2 API](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/path/list) 枚举文件列表。 请将以下示例作为指南来查看。

```csharp
public void ListBlobsInContainer(string containerName, string iothub)
{
    var storageAccount = CloudStorageAccount.Parse(this.blobConnectionString);
    var cloudBlobContainer = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
    if (cloudBlobContainer.Exists())
    {
        var results = cloudBlobContainer.ListBlobs(prefix: $"{iothub}/");
        foreach (IListBlobItem item in results)
        {
            Console.WriteLine(item.Uri);
        }
    }
}
```

若要创建与 Azure Data Lake Gen2 兼容的存储帐户，请创建新的 V2 存储帐户，并在“高级”选项卡的“分层命名空间”字段上选择“启用”，如下图所示：

![选择 Azure Date Lake Gen2 存储](./media/iot-hub-devguide-messages-d2c/selectadls2storage.png)


### <a name="service-bus-queues-and-service-bus-topics"></a>服务总线队列和服务总线主题

用作 IoT 中心终结点的服务总线队列和主题不能启用“会话”或“重复项检测”。 如果启用了其中任一选项，该终结点将在 Azure 门户中显示为“无法访问”。

### <a name="event-hubs"></a>事件中心

除了与事件中心兼容的内置终结点外，还可以将数据路由到事件中心类型的自定义终结点。 

## <a name="reading-data-that-has-been-routed"></a>读取已路由的数据

可以按照此[教程](tutorial-routing.md)配置一个路由。

使用以下教程了解如何从终结点读取消息。

* 从[内置终结点](quickstart-send-telemetry-node.md)进行读取

* 从 [Blob 存储](../storage/blobs/storage-blob-event-quickstart.md)进行读取

* 从[事件中心](../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)进行读取

* 从[服务总线队列](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)进行读取

* 从[服务总线主题](/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)进行读取

## <a name="fallback-route"></a>回退路由

回退路由将所有不满足任何现有路由上的查询条件的消息发送到与[事件中心](/event-hubs/)兼容的内置事件中心（消息/事件）。 如果已启用消息路由，则可以启用此回退路由功能。 在创建一个路由后，数据将停止流向内置终结点，除非创建了到该终结点的路由。 如果没有到内置终结点的路由并且已启用回退路由，则仅与路由上的任何查询条件不匹配的消息将被发送到内置终结点。 此外，如果已删除现有路由，必须启用回退路由才能接收内置终结点处的所有数据。 

可以在 Azure 门户->“消息路由”边栏选项卡中启用/禁用回退路由。 还可以将 Azure 资源管理器用于 [FallbackRouteProperties](https://docs.microsoft.com/en-us/rest/api/iothub/iothubresource/createorupdate#fallbackrouteproperties) 来为回退路由使用自定义终结点。

## <a name="non-telemetry-events"></a>非遥测事件

除了设备遥测数据之外，消息路由还支持发送设备孪生更改事件、设备生命周期事件和数字孪生更改事件（在公共预览版中）。 例如，如果使用数据源创建一个设置为到**设备孪生更改事件**的路由，IoT 中心会将消息发送到包含设备孪生更改的终结点。 同样，如果创建路由时将数据源设置为“设备生命周期事件”，则 IoT 中心会发送一条消息，指示是否删除或创建了设备。 

[IoT 中心还集成了 Azure 事件网格](iot-hub-event-grid.md)来发布设备事件以支持基于这些事件的工作流的实时集成和自动化。 请参阅[消息路由和事件网格之间的主要区别](iot-hub-event-grid-routing-comparison.md)来了解哪种更适合你的方案。

## <a name="testing-routes"></a>测试路由

在创建新路由或编辑现有路由时，应通过示例消息来测试路由查询。 可以测试单个路由或一次测试所有路由，并且在测试期间，不会有消息被路由到终结点。 可以使用 Azure 门户、Azure 资源管理器、Azure PowerShell 和 Azure CLI 来进行测试。 测试结果有助于确定示例消息是否与查询相匹配，或者测试是否因为示例消息或查询语法错误而无法运行。 若要了解详细信息，请参阅[测试路由](https://docs.microsoft.com/en-us/rest/api/iothub/iothubresource/testroute)和[测试所有路由](https://docs.microsoft.com/en-us/rest/api/iothub/iothubresource/testallroutes)。
## <a name="ordering-guarantees-with-at-least-once-delivery"></a>排序保证至少传送一次

IoT 中心消息路由可保证按顺序至少将消息传送到终结点一次。 这意味着可以存在重复的消息，并且可以按照原始消息顺序重新传输一系列消息。 例如，如果原始消息顺序是 [1,2,3,4]，则可能会收到类似 [1,2,1,2,3,1,2,3,4] 的消息序列。 排序保证指的是，如果收到过消息 [1]，则其后总是 [2,3,4]。

为了处理消息重复项，我们建议在原点（通常是设备或模块）的消息的应用程序属性中标记一个唯一标识符。 传送消息的服务可以使用此标识符来处理重复的消息。

## <a name="latency"></a>延迟

使用内置终结点路由设备到云遥测消息时，在创建第一个路由后，端到端延迟略微增大。

在大多数情况下，延迟的平均增大量小于 500 毫秒。 可以使用路由：消息/事件的消息延迟或 d2c.endpoints.latency.builtIn.events IoT 中心指标来监视延迟 。 在创建第一个路由后创建或删除任何路由不会影响端到端延迟。

## <a name="monitoring-and-troubleshooting"></a>监视和故障排除

IoT 中心提供了多个与路由和终结点相关的指标，使你能够大致了解你的中心的运行状况和已发送的消息数。 [IoT 中心指标](iot-hub-metrics.md)列出了默认为 IoT 中心启用的所有指标。 通过使用 Azure Monitor [诊断设置](../iot-hub/iot-hub-monitor-resource-health.md)中的路由诊断日志，可以跟踪发生在路由查询和终结点运行状况的评估期间、由 IoT 中心所察觉到的错误。 可以使用 REST API [Get Endpoint Health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) 获取终结点的[运行状况状态](iot-hub-devguide-endpoints.md#custom-endpoints)。 

通过[路由故障排除指南](troubleshoot-message-routing.md)获取更多详细信息以及对路由故障排除的支持。

## <a name="next-steps"></a>后续步骤

* 若要了解如何创建消息路由，请参阅[使用路由处理 IoT 中心的设备到云消息](tutorial-routing.md)。

* [如何发送设备到云消息](quickstart-send-telemetry-node.md)

* 有关可以用来发送设备到云消息的 SDK 的详细信息，请参阅 [Azure IoT SDK](iot-hub-devguide-sdks.md)。
