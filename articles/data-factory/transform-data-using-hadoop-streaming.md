---
title: 使用 Hadoop 流式处理活动转换数据
description: 说明如何使用 Azure 数据工厂中的 Hadoop 流式处理活动通过在 HDInsight 群集上运行 Hadoop 流式处理程序来转换数据。
author: WenJason
ms.author: v-jay
manager: digimobile
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
origin.date: 05/08/2020
ms.date: 06/29/2020
ms.openlocfilehash: 8c01e8dea65d9d3bc6533014ec458927f1f89978
ms.sourcegitcommit: f5484e21fa7c95305af535d5a9722b5ab416683f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85323109"
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>使用 Azure 数据工厂中的 Hadoop 流式处理活动转换数据

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

数据工厂[管道](concepts-pipelines-activities.md)中的 HDInsight 流式处理活动会在[自己的](compute-linked-services.md#azure-hdinsight-linked-service)或[按需](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight 群集上执行 Hadoop 流式处理程序。 本文基于[数据转换活动](transform-data.md)一文，它概述了数据转换和受支持的转换活动。

如果不熟悉 Azure 数据工厂，请在阅读本文之前，先通读 [Azure 数据工厂简介](introduction.md)，并学习[教程：转换数据](tutorial-transform-data-spark-powershell.md)。 

## <a name="json-sample"></a>JSON 示例
```json
{
    "name": "Streaming Activity",
    "description": "Description",
    "type": "HDInsightStreaming",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mapper": "MyMapper.exe",
        "reducer": "MyReducer.exe",
        "combiner": "MyCombiner.exe",
        "fileLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "filePaths": [
            "<containername>/example/apps/MyMapper.exe",
            "<containername>/example/apps/MyReducer.exe",
            "<containername>/example/apps/MyCombiner.exe"
        ],
        "input": "wasb://<containername>@<accountname>.blob.core.chinacloudapi.cn/example/input/MapperInput.txt",
        "output": "wasb://<containername>@<accountname>.blob.core.chinacloudapi.cn/example/output/ReducerOutput.txt",
        "commandEnvironment": [
            "CmdEnvVarName=CmdEnvVarValue"
        ],
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }
}
```

## <a name="syntax-details"></a>语法详细信息

| 属性          | 说明                              | 必须 |
| ----------------- | ---------------------------------------- | -------- |
| name              | 活动名称                     | 是      |
| description       | 描述活动用途的文本 | 否       |
| type              | 对于 Hadoop 流式处理活动，活动类型是 HDInsightStreaming | 是      |
| linkedServiceName | 引用在数据工厂中注册为链接服务的 HDInsight 群集。 若要了解此链接服务，请参阅[计算链接服务](compute-linked-services.md)一文。 | 是      |
| mapper            | 指定映射器可执行文件的名称 | 是      |
| reducer           | 指定化简器可执行文件的名称 | 是      |
| combiner          | 指定合并器可执行文件的名称 | 否       |
| fileLinkedService | 对 Azure 存储链接服务的引用，该服务用于存储要执行的映射器、合并器和化简器程序。 此处仅支持 [Azure Blob 存储](/data-factory/connector-azure-blob-storage)和 [ADLS Gen2](/data-factory/connector-azure-data-lake-storage) 链接服务 。 如果未指定此链接服务，则使用 HDInsight 链接服务中定义的 Azure 存储链接服务。 | 否       |
| filePath          | 提供由 fileLinkedService 引用的 Azure 存储中存储的映射器、合并器和化简器程序的路径数组。 此路径区分大小写。 | 是      |
| input             | 指定映射器输入文件的 WASB 路径。 | 是      |
| output            | 指定化简器输出文件的 WASB 路径。 | 是      |
| getDebugInfo      | 指定何时将日志文件复制到 HDInsight 群集使用的（或者）scriptLinkedService 指定的 Azure 存储。 允许的值：None、Always 或 Failure。 默认值：无。 | 否       |
| 参数         | 指定 Hadoop 作业的参数数组。 参数以命令行参数的形式传递到每个任务。 | 否       |
| 定义           | 在 Hive 脚本中指定参数作为键/值对，以供引用。 | 否       | 

## <a name="next-steps"></a>后续步骤
参阅以下文章了解如何以其他方式转换数据： 

* [Hive 活动](transform-data-using-hadoop-hive.md)
* [Pig 活动](transform-data-using-hadoop-pig.md)
* [MapReduce 活动](transform-data-using-hadoop-map-reduce.md)
* [Spark 活动](transform-data-using-spark.md)
* [.NET 自定义活动](transform-data-using-dotnet-custom-activity.md)
* [存储过程活动](transform-data-using-stored-procedure.md)
