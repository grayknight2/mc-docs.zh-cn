---
title: 用于 Azure 警报中日志警报的 Webhook 操作
description: 本文介绍如何使用 Log Analytics 工作区或 Application Insights 创建日志警报规则，警报如何作为 HTTP Webhook 推送数据，以及可能的不同自定义设置的详细信息。
author: Johnnytechn
ms.author: v-johya
services: monitoring
ms.topic: conceptual
origin.date: 05/30/2019
ms.date: 07/17/2020
ms.subservice: alerts
ms.openlocfilehash: 13fef7d93e3571a9b9cfaeadaf6bfd074839bae3
ms.sourcegitcommit: b5794af488a336d84ee586965dabd6f45fd5ec6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87508459"
---
# <a name="webhook-actions-for-log-alert-rules"></a>用于日志警报规则的 Webhook 操作
[在 Azure 中创建日志警报](alerts-log.md)时，可以选择[使用操作组配置](action-groups.md)以执行一个或多个操作。 本文介绍可用的不同 Webhook 操作，以及如何配置基于 JSON 的自定义 Webhook。

> [!NOTE]
> 还可以使用[通用警报架构](https://aka.ms/commonAlertSchemaDocs)进行 Webhook 集成。 常见警报架构具有以下优势：跨 Azure Monitor 中的所有警报服务提供单个可扩展且统一的警报有效负载。请注意，常见警报架构不支持日志警报的自定义 JSON 选项。 如果选择了此选项，则它将遵从常见警报架构有效负载，而不考虑在警报规则级别执行的自定义。 [了解常见的警报架构定义。](https://aka.ms/commonAlertSchemaDefinitions)

## <a name="webhook-actions"></a>Webhook 操作

使用 Webhook 操作可通过单个 HTTP POST 请求调用外部进程。 被调用的服务应支持 Webhook，并确定将如何使用接收的任何有效负载。

Webhook 操作需要下表中的属性。

| 属性 | 说明 |
|:--- |:--- |
| **Webhook URL** |Webhook 的 URL。 |
| **自定义 JSON 有效负载** |如果在创建警报期间选择了此选项，请自定义要通过 webhook 发送的有效负载。 有关详细信息，请参阅[管理日志警报](alerts-log.md)。|

> [!NOTE]
> 单击日志警报的“包括 Webhook 的自定义 JSON 有效负载”选项旁边的“查看 Webhook”按钮会显示所提供的自定义的示例 Webhook 有效负载。 它不包含实际数据，也不代表用于日志警报的 JSON 架构。 

Webhooks 包括 URL 和 JSON 格式的有效负载（即发送到外部服务的数据）。 默认情况下，有效负载包括下表中的值。 可以选择将此负载替换成自己的自定义负载。 在这种情况下，可以使用下表中每个参数的变量，将其值包含在自定义有效负载中。


| 参数 | 变量 | 说明 |
|:--- |:--- |:--- |
| *AlertRuleName* |#alertrulename |警报规则的名称。 |
| *严重性* |#severity |为触发的日志警报设置的严重性。 |
| *AlertThresholdOperator* |#thresholdoperator |警报规则的阈值运算符，使用“大于”或“小于”。 |
| *AlertThresholdValue* |#thresholdvalue |警报规则的阈值。 |
| *LinkToSearchResults* |#linktosearchresults |指向 Analytics 门户的链接，该门户会从创建警报的查询返回记录。 |
| LinkToSearchResultsAPI |#linktosearchresultsapi |指向 Analytics API 的链接，该 API 会从创建警报的查询返回记录。 |
| LinkToFilteredSearchResultsUI |#linktofilteredsearchresultsui |指向 Analytics 门户的链接，该门户返回按创建警报的维度值组合筛选的查询中的记录。 |
| LinkToFilteredSearchResultsAPI |#linktofilteredsearchresultsapi |指向 Analytics API 的链接，该 API 返回按创建警报的维度值组合筛选的查询中的记录。 |
| *ResultCount* |#searchresultcount |搜索结果中的记录数。 |
| 搜索时间间隔结束时间 |#searchintervalendtimeutc |查询结束时间 (UTC)，格式为 mm/dd/yyyy HH:mm:ss AM/PM。 |
| 搜索时间间隔 |#searchinterval |警报规则的时间范围，格式为 HH:mm:ss。 |
| 搜索时间间隔开始时间 |#searchintervalstarttimeutc |查询开始时间 (UTC)，格式为 mm/dd/yyyy HH:mm:ss AM/PM。 
| *SearchQuery* |#searchquery |警报规则所使用的日志搜索查询。 |
| *SearchResults* |"IncludeSearchResults": true|查询以 JSON 表形式返回的记录，仅限于前 1,000 条记录。 在自定义 JSON Webhook 定义中添加 "IncludeSearchResults": true 作为顶级属性。 |
| *Dimensions* |"IncludeDimensions": true|将该警报作为 JSON 部分触发的维度值组合。 在自定义 JSON Webhook 定义中添加 "IncludeDimensions": true 作为顶级属性。 |
| 警报类型| #alerttype | 配置为[指标度量](alerts-unified-log.md#metric-measurement-alert-rules) 或 [结果数](alerts-unified-log.md#number-of-results-alert-rules)的日志警报规则的类型。|
| *WorkspaceID* |#workspaceid |Log Analytics 工作区的 ID。 |
| *应用程序 ID* |#applicationid |Application Insights 应用的 ID。 |
| *订阅 ID* |#subscriptionid |使用的 Azure 订阅的 ID。 

> [!NOTE]
> 给定链接将 URL 中的参数（如 SearchQuery、“搜索时间间隔开始时间”和“搜索时间间隔结束时间”）传递到 Azure 门户或 API  。

例如，可以指定以下自定义负载，其中包含名为 *text* 的单一参数。 此 Webhook 调用的服务需要此参数。

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
此示例的有效负载会在发送到 Webhook 时解析如下：

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```
由于自定义 Webhook 中的所有变量都必须在 JSON enclosure（如“#searchinterval”）内指定，因此生成的 Webhook 在 enclosure（如“00:05:00”）内也会有可变数据。

若要在自定义有效负载中包含搜索结果，请确保在 JSON 有效负载中将 **IncludeSearchResults** 设置为顶级属性。 

## <a name="sample-payloads"></a>示例有效负载
本部分显示用于日志警报的 Webhook 的示例有效负载。 示例有效负载包括有效负载是标准有效负载时以及是自定义有效负载时的示例。

### <a name="standard-webhook-for-log-alerts"></a>用于日志警报的标准 Webhook 
这两个示例是仅包含两列和两行的虚拟有效负载。

#### <a name="log-alert-for-log-analytics"></a>Log Analytics 的日志警报
以下示例有效负载适用于基于 Log Analytics 的警报使用的不带自定义 JSON 选项的标准 Webhook 操作。

```json
{
    "SubscriptionId": "12345a-1234b-123c-123d-12345678e",
    "AlertRuleName": "AcmeRule",
    "SearchQuery": "Perf | where ObjectName == \"Processor\" and CounterName == \"% Processor Time\" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer",
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://portal.azure.cn/#Analyticsblade/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "LinkToFilteredSearchResultsUI": "https://portal.azure.cn/#Analyticsblade/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "LinkToSearchResultsAPI": "https://api.loganalytics.io/v1/workspaces/workspaceID/query?query=Heartbeat&timespan=2020-05-07T18%3a11%3a51.0000000Z%2f2020-05-07T18%3a16%3a51.0000000Z",
    "LinkToFilteredSearchResultsAPI": "https://api.loganalytics.io/v1/workspaces/workspaceID/query?query=Heartbeat&timespan=2020-05-07T18%3a11%3a51.0000000Z%2f2020-05-07T18%3a16%3a51.0000000Z",
    "Description": "log alert rule",
    "Severity": "Warning",
    "AffectedConfigurationItems": [
        "INC-Gen2Alert"
    ],
    "Dimensions": [
        {
            "name": "Computer",
            "value": "INC-Gen2Alert"
        }
    ],
    "SearchResult": {
        "tables": [
            {
                "name": "PrimaryResult",
                "columns": [
                    {
                        "name": "$table",
                        "type": "string"
                    },
                    {
                        "name": "Computer",
                        "type": "string"
                    },
                    {
                        "name": "TimeGenerated",
                        "type": "datetime"
                    }
                ],
                "rows": [
                    [
                        "Fabrikam",
                        "33446677a",
                        "2018-02-02T15:03:12.18Z"
                    ],
                    [
                        "Contoso",
                        "33445566b",
                        "2018-02-02T15:16:53.932Z"
                    ]
                ]
            }
        ]
    },
    "WorkspaceId": "12345a-1234b-123c-123d-12345678e",
    "AlertType": "Metric measurement"
}
 ```

<!--Not available in MC: alerts-log-api-switch.md-->
#### <a name="log-alert-for-application-insights"></a>Application Insights 的日志警报
以下示例有效负载是适用于基于 Application Insights 的日志警报使用的不带自定义 JSON 选项的标准 Webhook。
    
```json
{
    "schemaId": "Microsoft.Insights/LogAlert",
    "data": {
        "SubscriptionId": "12345a-1234b-123c-123d-12345678e",
        "AlertRuleName": "AcmeRule",
        "SearchQuery": "requests | where resultCode == \"500\" | summarize AggregatedValue = Count by bin(Timestamp, 5m), IP",
        "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
        "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
        "AlertThresholdOperator": "Greater Than",
        "AlertThresholdValue": 0,
        "ResultCount": 2,
        "SearchIntervalInSeconds": 3600,
        "LinkToSearchResults": "https://portal.azure.cn/AnalyticsBlade/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
        "LinkToFilteredSearchResultsUI": "https://portal.azure.cn/AnalyticsBlade/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
        "LinkToSearchResultsAPI": "https://api.applicationinsights.io/v1/apps/0MyAppId0/metrics/requests/count",
        "LinkToFilteredSearchResultsAPI": "https://api.applicationinsights.io/v1/apps/0MyAppId0/metrics/requests/count",
        "Description": null,
        "Severity": "3",
        "Dimensions": [
            {
                "name": "IP",
                "value": "1.1.1.1"
            }
        ],
        "SearchResult": {
            "tables": [
                {
                    "name": "PrimaryResult",
                    "columns": [
                        {
                            "name": "$table",
                            "type": "string"
                        },
                        {
                            "name": "Id",
                            "type": "string"
                        },
                        {
                            "name": "Timestamp",
                            "type": "datetime"
                        }
                    ],
                    "rows": [
                        [
                            "Fabrikam",
                            "33446677a",
                            "2018-02-02T15:03:12.18Z"
                        ],
                        [
                            "Contoso",
                            "33445566b",
                            "2018-02-02T15:16:53.932Z"
                        ]
                    ]
                }
            ]
        },
        "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1",
        "AlertType": "Metric measurement"
    }
}
```

#### <a name="log-alert-with-custom-json-payload"></a>带自定义 JSON 有效负载的日志警报
例如，若要创建只包含警报名称和搜索结果的自定义有效负载，可以使用以下代码： 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

下面是用于任何日志警报的自定义 Webhook 操作的示例有效负载：
    
```json
    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResults":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }
```


## <a name="next-steps"></a>后续步骤
- 了解 [Azure 警报中的日志警报](alerts-unified-log.md)。
- 了解如何[管理 Azure 中的日志警报](alerts-log.md)。
- 创建和管理 [Azure 中的操作组](action-groups.md)。
- 详细了解 [Application Insights](../../azure-monitor/log-query/log-query-overview.md)。
- 了解有关[日志查询](../log-query/log-query-overview.md)的详细信息。 


