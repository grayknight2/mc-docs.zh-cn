---
title: include 文件
description: include 文件
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 08/07/2020
ms.topic: include
ms.custom: include file, devx-track-javascript
ms.author: v-johya
ms.openlocfilehash: 2e8cf61909b0c4c9d595a973478f9009108ccb51
ms.sourcegitcommit: caa18677adb51b5321ad32ae62afcf92ac00b40b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2020
ms.locfileid: "88024225"
---
使用适用于 Node.js 的语言理解 (LUIS) 创作客户端库可以：

* 创建应用。
* 添加意向、实体和示例话语。
* 添加短语列表等特征。
* 训练和发布应用。
* 删除应用

[参考文档](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/?view=azure-node-latest) | [库源代码](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-luis-authoring) | [创作包 (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-authoring)、[运行时包 (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime) | [示例](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js)

## <a name="prerequisites"></a>先决条件

* 语言理解创作资源：[在 Azure 门户中创建一个](https://portal.azure.cn/#create/Microsoft.CognitiveServicesLUISAllInOne)
* [Node.js](https://nodejs.org)

## <a name="setting-up"></a>设置

### <a name="get-your-language-understanding-luis-starter-key"></a>获取语言理解 (LUIS) 初学者密钥

通过创建 LUIS 创作资源，获取[创作密钥](../luis-how-to-azure-subscription.md)。 保留密钥和密钥的终结点，你需要在代码文件的顶部将这些内容添加为字符串。

### <a name="install-the-npm-library-for-luis-authoring"></a>安装适用于 LUIS 创作的 NPM 库

在应用程序目录中，使用以下命令安装依赖项：

```console
npm install @azure/cognitiveservices-luis-authoring @azure/ms-rest-js
```

## <a name="object-model"></a>对象模型

语言理解 (LUIS) 创作客户端是对 Azure 进行身份验证的 [LUISAuthoringClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/luisauthoringclient?view=azure-node-latest) 对象，其中包含创作密钥。

创建客户端后，可以使用此客户端访问如下所述的功能：

* 应用 - [添加](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#add-applicationcreateobject--msrest-requestoptionsbase-)、[删除](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#deletemethod-string--models-appsdeletemethodoptionalparams-)、[发布](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#publish-string--applicationpublishobject--msrest-requestoptionsbase-)
* 示例言语 - [按批添加](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examples?view=azure-node-latest#batch-string--string--examplelabelobject----msrest-requestoptionsbase-)、[按 ID 删除](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examples?view=azure-node-latest#deletemethod-string--string--number--msrest-requestoptionsbase-)
* 特征 - 管理[短语列表](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/features?view=azure-node-latest#addphraselist-string--string--phraselistcreateobject--msrest-requestoptionsbase-)
* 模型 - 管理[意向](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest#addintent-string--string--modelcreateobject--msrest-requestoptionsbase-)和实体
* 模式 - 管理[模式](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/pattern?view=azure-node-latest#addpattern-string--string--patternrulecreateobject--msrest-requestoptionsbase-)
* 训练 - [训练](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#trainversion-string--string--msrest-requestoptionsbase-)应用和轮询[训练状态](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#getstatus-string--string--msrest-requestoptionsbase-)
* [版本](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/versions?view=azure-node-latest) - 使用克隆、导出和删除操作进行管理


## <a name="code-examples"></a>代码示例

这些代码片段展示了如何使用适用于 Node.js 的语言理解 (LUIS) 创作客户端库来执行以下操作：

* [创建应用](#create-a-luis-app)
* [添加实体](#create-entities-for-the-app)
* [添加意向](#create-intent-for-the-app)
* [添加示例表述](#add-example-utterance-to-intent)
* [训练应用](#train-the-app)
* [发布应用](#publish-a-language-understanding-app)
* [删除应用](#delete-a-language-understanding-app)
* [列出应用](#list-language-understanding-apps)

## <a name="create-a-new-nodejs-application"></a>创建新的 Node.js 应用程序

在你喜欢使用的编辑器或 IDE 中创建名为 `luis_authoring_quickstart.js` 的新文本文件。 然后，添加以下依赖项。

```javascript
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Create an application.
 * - Add intents to an application.
 * - Add entities to an application.
 * - Add utterances to an application.
 * - Train an application.
 * - Publish an application.
 * - Delete an application.
 * - List all applications.
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-authoring
 */
// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-authoring");
// </Dependencies>

// <Variables>
const key = 'REPLACE-WITH-YOUR-ASSIGNED-AUTHORING-KEY';

const endpoint = "https://REPLACE-WITH-RESOURCE-NAME.cognitiveservices.azure.cn/"
// </Variables>

// <AuthoringCreateClient>
const luisAuthoringCredentials = new msRest.ApiKeyCredentials({
  inHeader: { "Ocp-Apim-Subscription-Key": key }
});
const luisAuthoringClient = new LUIS.LUISAuthoringClient(
  luisAuthoringCredentials,
  endpoint
);
// </AuthoringCreateClient>

// <utilForQuickstartOnlyToSimulateAsyncOperation >
const delayTimer = async timeInMs => {
  return await new Promise(resolve => {
    setTimeout(resolve, timeInMs);
  });
};
// </utilForQuickstartOnlyToSimulateAsyncOperation >

// <AuthoringCreateApplication>
const create_app = async () => {
  const create_app_payload = {
    name: "Contoso",
    description: "Flight booking app built with Azure SDK for Java.",
    initialVersionId: "0.1",
    culture: "en-us"
  };

  const createAppResult = await luisAuthoringClient.apps.add(
    create_app_payload
  );

  console.log(`Created LUIS app with ID ${createAppResult.body}`);

  return {
    id: createAppResult.body,
    version: create_app_payload.initialVersionId
  };
}
// </AuthoringCreateApplication>

// <AuthoringAddEntities>
const add_entities = async app_info => {
  const addEntityDestinationResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Destination" }
  );
  console.log("Entity Destination created.");

  const addEntityClassResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Class" }
  );
  console.log("Entity Class created.");

  const addEntityFlightResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Flight" }
  );
  console.log("Entity Flight created.");
}
// </AuthoringAddEntities>

// <AuthoringAddIntents>
const add_intents = async app_info => {
  const addIntentFindFlightsResult = await luisAuthoringClient.model.addIntent(
    app_info.id,
    app_info.version,
    { name: "FindFlights" }
  );
  console.log("Intent FindFlights added.");
};
// </AuthoringAddIntents>

// <AuthoringBatchAddUtterancesForIntent>
const create_utterance = (intent, text, labels) => {
  var entityLabels = new Array();

  labels.forEach((value, key, map) => {
    const start_index = text.toLowerCase().indexOf(value.toLowerCase());
    const end_index = start_index + value.length;

    if (start_index > -1) {
      entityLabels.push({
        entityName: key,
        startCharIndex: start_index,
        endCharIndex: end_index
      });
    }
  });

  console.log(`Created ${entityLabels.length} entity labels.`);

  return { text: text, entityLabels: entityLabels, intentName: intent };
}

const add_utterances = async app_info => {
  const utterance_1 = create_utterance(
    "FindFlights",
    "find flights in economy to Madrid",
    new Map([
      ["Flight", "economy to Madrid"],
      ["Destination", "Madrid"],
      ["Class", "economy"]
    ])
  );
  const utterance_2 = create_utterance(
    "FindFlights",
    "find flights to London in first class",
    new Map([
      ["Flight", "London in first class"],
      ["Destination", "London"],
      ["Class", "first"]
    ])
  );
  const utterance_3 = create_utterance(
    "FindFlights",
    "find flights from seattle to London in first class",
    new Map([
      ["Flight", "flights from seattle to London in first class"],
      ["Location", "London"],
      ["Location", "Seattle"],
      ["Class", "first"]
    ])
  );

  const luisExamplesBatchResult = await luisAuthoringClient.examples.batch(
    app_info.id,
    app_info.version,
    [utterance_1, utterance_2, utterance_3]
  );
  console.log("Example utterances added.");
}
// </AuthoringBatchAddUtterancesForIntent>

// <AuthoringWaitForOperation>
const wait_for_operation = async app_info => {
  let operationResult = null;
  let modelUniqueStatus = ["InProgress"];

  while (
    modelUniqueStatus.includes("InProgress") ||
    modelUniqueStatus.includes("Queued")
  ) {
    await delayTimer(1000);

    operationResult = await luisAuthoringClient.train.getStatus(
      app_info.id,
      app_info.version
    );

    modelUniqueStatus = [
      ...new Set(operationResult.map(op => op.details.status))
    ];

    console.log(`Current model status: ${JSON.stringify(modelUniqueStatus)}`);
  }

  return operationResult;
}
// </AuthoringWaitForOperation>

// <AuthoringTrainVersion>
const train_app = async app_info => {
  const trainResult = await luisAuthoringClient.train.trainVersion(
    app_info.id,
    app_info.version
  );

  console.log("Waiting for train operation to finish...");

  const operationResult = await wait_for_operation(app_info);
}
// </AuthoringTrainVersion>

// <AuthoringPublishVersion>
const publish_app = async app_info => {
  const publishResult = await luisAuthoringClient.apps.publish(app_info.id, {
    versionId: app_info.version,
    isStaging: true
  });

  console.log(
    `Application published. Endpoint URL: ${publishResult.endpointUrl}`
  );
}
// </AuthoringPublishVersion>

// <AuthoringDeleteApp>
const delete_app = async app_info => {
  const deleteResult = await luisAuthoringClient.apps.deleteMethod(app_info.id);

  console.log(
    `Application with ID ${app_info.id} deleted. Operation result: ${deleteResult.message}`
  );
}
// </AuthoringDeleteApp>

// <AuthoringListApps>
const list_apps = async () => {
  const apps = await luisAuthoringClient.apps.list;

  for (let app of apps) {
    console.log(`ID ${app.id}, NAME ${app.name}`);
  }
}
// </AuthoringListApps>

// <Main>
const quickstart = async () => {
  //    list_apps();
  var app_info = await create_app();
  await add_entities(app_info);
  await add_intents(app_info);
  await add_utterances(app_info);
  await train_app(app_info);
  await publish_app(app_info);
  await delete_app(app_info);
}

try {
  quickstart();
} catch (error) {
  console.log(error);
}
// </Main>
```

```javascript
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Create an application.
 * - Add intents to an application.
 * - Add entities to an application.
 * - Add utterances to an application.
 * - Train an application.
 * - Publish an application.
 * - Delete an application.
 * - List all applications.
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-authoring
 */
// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-authoring");
// </Dependencies>

// <Variables>
const key = 'REPLACE-WITH-YOUR-ASSIGNED-AUTHORING-KEY';

const endpoint = "https://REPLACE-WITH-RESOURCE-NAME.cognitiveservices.azure.cn/"
// </Variables>

// <AuthoringCreateClient>
const luisAuthoringCredentials = new msRest.ApiKeyCredentials({
  inHeader: { "Ocp-Apim-Subscription-Key": key }
});
const luisAuthoringClient = new LUIS.LUISAuthoringClient(
  luisAuthoringCredentials,
  endpoint
);
// </AuthoringCreateClient>

// <utilForQuickstartOnlyToSimulateAsyncOperation >
const delayTimer = async timeInMs => {
  return await new Promise(resolve => {
    setTimeout(resolve, timeInMs);
  });
};
// </utilForQuickstartOnlyToSimulateAsyncOperation >

// <AuthoringCreateApplication>
const create_app = async () => {
  const create_app_payload = {
    name: "Contoso",
    description: "Flight booking app built with Azure SDK for Java.",
    initialVersionId: "0.1",
    culture: "en-us"
  };

  const createAppResult = await luisAuthoringClient.apps.add(
    create_app_payload
  );

  console.log(`Created LUIS app with ID ${createAppResult.body}`);

  return {
    id: createAppResult.body,
    version: create_app_payload.initialVersionId
  };
}
// </AuthoringCreateApplication>

// <AuthoringAddEntities>
const add_entities = async app_info => {
  const addEntityDestinationResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Destination" }
  );
  console.log("Entity Destination created.");

  const addEntityClassResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Class" }
  );
  console.log("Entity Class created.");

  const addEntityFlightResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Flight" }
  );
  console.log("Entity Flight created.");
}
// </AuthoringAddEntities>

// <AuthoringAddIntents>
const add_intents = async app_info => {
  const addIntentFindFlightsResult = await luisAuthoringClient.model.addIntent(
    app_info.id,
    app_info.version,
    { name: "FindFlights" }
  );
  console.log("Intent FindFlights added.");
};
// </AuthoringAddIntents>

// <AuthoringBatchAddUtterancesForIntent>
const create_utterance = (intent, text, labels) => {
  var entityLabels = new Array();

  labels.forEach((value, key, map) => {
    const start_index = text.toLowerCase().indexOf(value.toLowerCase());
    const end_index = start_index + value.length;

    if (start_index > -1) {
      entityLabels.push({
        entityName: key,
        startCharIndex: start_index,
        endCharIndex: end_index
      });
    }
  });

  console.log(`Created ${entityLabels.length} entity labels.`);

  return { text: text, entityLabels: entityLabels, intentName: intent };
}

const add_utterances = async app_info => {
  const utterance_1 = create_utterance(
    "FindFlights",
    "find flights in economy to Madrid",
    new Map([
      ["Flight", "economy to Madrid"],
      ["Destination", "Madrid"],
      ["Class", "economy"]
    ])
  );
  const utterance_2 = create_utterance(
    "FindFlights",
    "find flights to London in first class",
    new Map([
      ["Flight", "London in first class"],
      ["Destination", "London"],
      ["Class", "first"]
    ])
  );
  const utterance_3 = create_utterance(
    "FindFlights",
    "find flights from seattle to London in first class",
    new Map([
      ["Flight", "flights from seattle to London in first class"],
      ["Location", "London"],
      ["Location", "Seattle"],
      ["Class", "first"]
    ])
  );

  const luisExamplesBatchResult = await luisAuthoringClient.examples.batch(
    app_info.id,
    app_info.version,
    [utterance_1, utterance_2, utterance_3]
  );
  console.log("Example utterances added.");
}
// </AuthoringBatchAddUtterancesForIntent>

// <AuthoringWaitForOperation>
const wait_for_operation = async app_info => {
  let operationResult = null;
  let modelUniqueStatus = ["InProgress"];

  while (
    modelUniqueStatus.includes("InProgress") ||
    modelUniqueStatus.includes("Queued")
  ) {
    await delayTimer(1000);

    operationResult = await luisAuthoringClient.train.getStatus(
      app_info.id,
      app_info.version
    );

    modelUniqueStatus = [
      ...new Set(operationResult.map(op => op.details.status))
    ];

    console.log(`Current model status: ${JSON.stringify(modelUniqueStatus)}`);
  }

  return operationResult;
}
// </AuthoringWaitForOperation>

// <AuthoringTrainVersion>
const train_app = async app_info => {
  const trainResult = await luisAuthoringClient.train.trainVersion(
    app_info.id,
    app_info.version
  );

  console.log("Waiting for train operation to finish...");

  const operationResult = await wait_for_operation(app_info);
}
// </AuthoringTrainVersion>

// <AuthoringPublishVersion>
const publish_app = async app_info => {
  const publishResult = await luisAuthoringClient.apps.publish(app_info.id, {
    versionId: app_info.version,
    isStaging: true
  });

  console.log(
    `Application published. Endpoint URL: ${publishResult.endpointUrl}`
  );
}
// </AuthoringPublishVersion>

// <AuthoringDeleteApp>
const delete_app = async app_info => {
  const deleteResult = await luisAuthoringClient.apps.deleteMethod(app_info.id);

  console.log(
    `Application with ID ${app_info.id} deleted. Operation result: ${deleteResult.message}`
  );
}
// </AuthoringDeleteApp>

// <AuthoringListApps>
const list_apps = async () => {
  const apps = await luisAuthoringClient.apps.list;

  for (let app of apps) {
    console.log(`ID ${app.id}, NAME ${app.name}`);
  }
}
// </AuthoringListApps>

// <Main>
const quickstart = async () => {
  //    list_apps();
  var app_info = await create_app();
  await add_entities(app_info);
  await add_intents(app_info);
  await add_utterances(app_info);
  await train_app(app_info);
  await publish_app(app_info);
  await delete_app(app_info);
}

try {
  quickstart();
} catch (error) {
  console.log(error);
}
// </Main>
```

## <a name="authenticate-the-client"></a>验证客户端

使用密钥创建 [CognitiveServicesCredentials](https://docs.microsoft.com/javascript/api/@azure/ms-rest-js/apikeycredentials?view=azure-node-latest) 对象，并在终结点中使用该对象创建一个 [LUISAuthoringClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/luisauthoringclient?view=azure-node-latest) 对象。

```javascript
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Create an application.
 * - Add intents to an application.
 * - Add entities to an application.
 * - Add utterances to an application.
 * - Train an application.
 * - Publish an application.
 * - Delete an application.
 * - List all applications.
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-authoring
 */
// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-authoring");
// </Dependencies>

// <Variables>
const key = 'REPLACE-WITH-YOUR-ASSIGNED-AUTHORING-KEY';

const endpoint = "https://REPLACE-WITH-RESOURCE-NAME.cognitiveservices.azure.cn/"
// </Variables>

// <AuthoringCreateClient>
const luisAuthoringCredentials = new msRest.ApiKeyCredentials({
  inHeader: { "Ocp-Apim-Subscription-Key": key }
});
const luisAuthoringClient = new LUIS.LUISAuthoringClient(
  luisAuthoringCredentials,
  endpoint
);
// </AuthoringCreateClient>

// <utilForQuickstartOnlyToSimulateAsyncOperation >
const delayTimer = async timeInMs => {
  return await new Promise(resolve => {
    setTimeout(resolve, timeInMs);
  });
};
// </utilForQuickstartOnlyToSimulateAsyncOperation >

// <AuthoringCreateApplication>
const create_app = async () => {
  const create_app_payload = {
    name: "Contoso",
    description: "Flight booking app built with Azure SDK for Java.",
    initialVersionId: "0.1",
    culture: "en-us"
  };

  const createAppResult = await luisAuthoringClient.apps.add(
    create_app_payload
  );

  console.log(`Created LUIS app with ID ${createAppResult.body}`);

  return {
    id: createAppResult.body,
    version: create_app_payload.initialVersionId
  };
}
// </AuthoringCreateApplication>

// <AuthoringAddEntities>
const add_entities = async app_info => {
  const addEntityDestinationResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Destination" }
  );
  console.log("Entity Destination created.");

  const addEntityClassResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Class" }
  );
  console.log("Entity Class created.");

  const addEntityFlightResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Flight" }
  );
  console.log("Entity Flight created.");
}
// </AuthoringAddEntities>

// <AuthoringAddIntents>
const add_intents = async app_info => {
  const addIntentFindFlightsResult = await luisAuthoringClient.model.addIntent(
    app_info.id,
    app_info.version,
    { name: "FindFlights" }
  );
  console.log("Intent FindFlights added.");
};
// </AuthoringAddIntents>

// <AuthoringBatchAddUtterancesForIntent>
const create_utterance = (intent, text, labels) => {
  var entityLabels = new Array();

  labels.forEach((value, key, map) => {
    const start_index = text.toLowerCase().indexOf(value.toLowerCase());
    const end_index = start_index + value.length;

    if (start_index > -1) {
      entityLabels.push({
        entityName: key,
        startCharIndex: start_index,
        endCharIndex: end_index
      });
    }
  });

  console.log(`Created ${entityLabels.length} entity labels.`);

  return { text: text, entityLabels: entityLabels, intentName: intent };
}

const add_utterances = async app_info => {
  const utterance_1 = create_utterance(
    "FindFlights",
    "find flights in economy to Madrid",
    new Map([
      ["Flight", "economy to Madrid"],
      ["Destination", "Madrid"],
      ["Class", "economy"]
    ])
  );
  const utterance_2 = create_utterance(
    "FindFlights",
    "find flights to London in first class",
    new Map([
      ["Flight", "London in first class"],
      ["Destination", "London"],
      ["Class", "first"]
    ])
  );
  const utterance_3 = create_utterance(
    "FindFlights",
    "find flights from seattle to London in first class",
    new Map([
      ["Flight", "flights from seattle to London in first class"],
      ["Location", "London"],
      ["Location", "Seattle"],
      ["Class", "first"]
    ])
  );

  const luisExamplesBatchResult = await luisAuthoringClient.examples.batch(
    app_info.id,
    app_info.version,
    [utterance_1, utterance_2, utterance_3]
  );
  console.log("Example utterances added.");
}
// </AuthoringBatchAddUtterancesForIntent>

// <AuthoringWaitForOperation>
const wait_for_operation = async app_info => {
  let operationResult = null;
  let modelUniqueStatus = ["InProgress"];

  while (
    modelUniqueStatus.includes("InProgress") ||
    modelUniqueStatus.includes("Queued")
  ) {
    await delayTimer(1000);

    operationResult = await luisAuthoringClient.train.getStatus(
      app_info.id,
      app_info.version
    );

    modelUniqueStatus = [
      ...new Set(operationResult.map(op => op.details.status))
    ];

    console.log(`Current model status: ${JSON.stringify(modelUniqueStatus)}`);
  }

  return operationResult;
}
// </AuthoringWaitForOperation>

// <AuthoringTrainVersion>
const train_app = async app_info => {
  const trainResult = await luisAuthoringClient.train.trainVersion(
    app_info.id,
    app_info.version
  );

  console.log("Waiting for train operation to finish...");

  const operationResult = await wait_for_operation(app_info);
}
// </AuthoringTrainVersion>

// <AuthoringPublishVersion>
const publish_app = async app_info => {
  const publishResult = await luisAuthoringClient.apps.publish(app_info.id, {
    versionId: app_info.version,
    isStaging: true
  });

  console.log(
    `Application published. Endpoint URL: ${publishResult.endpointUrl}`
  );
}
// </AuthoringPublishVersion>

// <AuthoringDeleteApp>
const delete_app = async app_info => {
  const deleteResult = await luisAuthoringClient.apps.deleteMethod(app_info.id);

  console.log(
    `Application with ID ${app_info.id} deleted. Operation result: ${deleteResult.message}`
  );
}
// </AuthoringDeleteApp>

// <AuthoringListApps>
const list_apps = async () => {
  const apps = await luisAuthoringClient.apps.list;

  for (let app of apps) {
    console.log(`ID ${app.id}, NAME ${app.name}`);
  }
}
// </AuthoringListApps>

// <Main>
const quickstart = async () => {
  //    list_apps();
  var app_info = await create_app();
  await add_entities(app_info);
  await add_intents(app_info);
  await add_utterances(app_info);
  await train_app(app_info);
  await publish_app(app_info);
  await delete_app(app_info);
}

try {
  quickstart();
} catch (error) {
  console.log(error);
}
// </Main>
```

## <a name="create-a-luis-app"></a>创建 LUIS 应用

1. 创建一个 LUIS 应用，用于包含保存意向、实体和示例言语的自然语言处理 (NLP) 模型。

1. 创建 [AppsOperation](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest) 对象的 [add](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest) 方法，用于创建应用。 名称和语言区域性是必需的属性。

```javascript
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Create an application.
 * - Add intents to an application.
 * - Add entities to an application.
 * - Add utterances to an application.
 * - Train an application.
 * - Publish an application.
 * - Delete an application.
 * - List all applications.
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-authoring
 */
// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-authoring");
// </Dependencies>

// <Variables>
const key = 'REPLACE-WITH-YOUR-ASSIGNED-AUTHORING-KEY';

const endpoint = "https://REPLACE-WITH-RESOURCE-NAME.cognitiveservices.azure.cn/"
// </Variables>

// <AuthoringCreateClient>
const luisAuthoringCredentials = new msRest.ApiKeyCredentials({
  inHeader: { "Ocp-Apim-Subscription-Key": key }
});
const luisAuthoringClient = new LUIS.LUISAuthoringClient(
  luisAuthoringCredentials,
  endpoint
);
// </AuthoringCreateClient>

// <utilForQuickstartOnlyToSimulateAsyncOperation >
const delayTimer = async timeInMs => {
  return await new Promise(resolve => {
    setTimeout(resolve, timeInMs);
  });
};
// </utilForQuickstartOnlyToSimulateAsyncOperation >

// <AuthoringCreateApplication>
const create_app = async () => {
  const create_app_payload = {
    name: "Contoso",
    description: "Flight booking app built with Azure SDK for Java.",
    initialVersionId: "0.1",
    culture: "en-us"
  };

  const createAppResult = await luisAuthoringClient.apps.add(
    create_app_payload
  );

  console.log(`Created LUIS app with ID ${createAppResult.body}`);

  return {
    id: createAppResult.body,
    version: create_app_payload.initialVersionId
  };
}
// </AuthoringCreateApplication>

// <AuthoringAddEntities>
const add_entities = async app_info => {
  const addEntityDestinationResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Destination" }
  );
  console.log("Entity Destination created.");

  const addEntityClassResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Class" }
  );
  console.log("Entity Class created.");

  const addEntityFlightResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Flight" }
  );
  console.log("Entity Flight created.");
}
// </AuthoringAddEntities>

// <AuthoringAddIntents>
const add_intents = async app_info => {
  const addIntentFindFlightsResult = await luisAuthoringClient.model.addIntent(
    app_info.id,
    app_info.version,
    { name: "FindFlights" }
  );
  console.log("Intent FindFlights added.");
};
// </AuthoringAddIntents>

// <AuthoringBatchAddUtterancesForIntent>
const create_utterance = (intent, text, labels) => {
  var entityLabels = new Array();

  labels.forEach((value, key, map) => {
    const start_index = text.toLowerCase().indexOf(value.toLowerCase());
    const end_index = start_index + value.length;

    if (start_index > -1) {
      entityLabels.push({
        entityName: key,
        startCharIndex: start_index,
        endCharIndex: end_index
      });
    }
  });

  console.log(`Created ${entityLabels.length} entity labels.`);

  return { text: text, entityLabels: entityLabels, intentName: intent };
}

const add_utterances = async app_info => {
  const utterance_1 = create_utterance(
    "FindFlights",
    "find flights in economy to Madrid",
    new Map([
      ["Flight", "economy to Madrid"],
      ["Destination", "Madrid"],
      ["Class", "economy"]
    ])
  );
  const utterance_2 = create_utterance(
    "FindFlights",
    "find flights to London in first class",
    new Map([
      ["Flight", "London in first class"],
      ["Destination", "London"],
      ["Class", "first"]
    ])
  );
  const utterance_3 = create_utterance(
    "FindFlights",
    "find flights from seattle to London in first class",
    new Map([
      ["Flight", "flights from seattle to London in first class"],
      ["Location", "London"],
      ["Location", "Seattle"],
      ["Class", "first"]
    ])
  );

  const luisExamplesBatchResult = await luisAuthoringClient.examples.batch(
    app_info.id,
    app_info.version,
    [utterance_1, utterance_2, utterance_3]
  );
  console.log("Example utterances added.");
}
// </AuthoringBatchAddUtterancesForIntent>

// <AuthoringWaitForOperation>
const wait_for_operation = async app_info => {
  let operationResult = null;
  let modelUniqueStatus = ["InProgress"];

  while (
    modelUniqueStatus.includes("InProgress") ||
    modelUniqueStatus.includes("Queued")
  ) {
    await delayTimer(1000);

    operationResult = await luisAuthoringClient.train.getStatus(
      app_info.id,
      app_info.version
    );

    modelUniqueStatus = [
      ...new Set(operationResult.map(op => op.details.status))
    ];

    console.log(`Current model status: ${JSON.stringify(modelUniqueStatus)}`);
  }

  return operationResult;
}
// </AuthoringWaitForOperation>

// <AuthoringTrainVersion>
const train_app = async app_info => {
  const trainResult = await luisAuthoringClient.train.trainVersion(
    app_info.id,
    app_info.version
  );

  console.log("Waiting for train operation to finish...");

  const operationResult = await wait_for_operation(app_info);
}
// </AuthoringTrainVersion>

// <AuthoringPublishVersion>
const publish_app = async app_info => {
  const publishResult = await luisAuthoringClient.apps.publish(app_info.id, {
    versionId: app_info.version,
    isStaging: true
  });

  console.log(
    `Application published. Endpoint URL: ${publishResult.endpointUrl}`
  );
}
// </AuthoringPublishVersion>

// <AuthoringDeleteApp>
const delete_app = async app_info => {
  const deleteResult = await luisAuthoringClient.apps.deleteMethod(app_info.id);

  console.log(
    `Application with ID ${app_info.id} deleted. Operation result: ${deleteResult.message}`
  );
}
// </AuthoringDeleteApp>

// <AuthoringListApps>
const list_apps = async () => {
  const apps = await luisAuthoringClient.apps.list;

  for (let app of apps) {
    console.log(`ID ${app.id}, NAME ${app.name}`);
  }
}
// </AuthoringListApps>

// <Main>
const quickstart = async () => {
  //    list_apps();
  var app_info = await create_app();
  await add_entities(app_info);
  await add_intents(app_info);
  await add_utterances(app_info);
  await train_app(app_info);
  await publish_app(app_info);
  await delete_app(app_info);
}

try {
  quickstart();
} catch (error) {
  console.log(error);
}
// </Main>
```


## <a name="create-intent-for-the-app"></a>为应用创建意向
LUIS 应用模型中的主要对象是意向。 意向与用户言语意向的分组相符。 用户可以提问，或者做出表述，指出希望机器人（或其他客户端应用程序）提供特定的有针对性响应。 意向的示例包括预订航班、询问目的地城市的天气，以及询问客户服务的联系信息。

将 [model.add_intent](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest#addintent-string--string--modelcreateobject--msrest-requestoptionsbase-) 方法与唯一意向的名称配合使用，然后传递应用 ID、版本 ID 和新的意向名称。

```javascript
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Create an application.
 * - Add intents to an application.
 * - Add entities to an application.
 * - Add utterances to an application.
 * - Train an application.
 * - Publish an application.
 * - Delete an application.
 * - List all applications.
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-authoring
 */
// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-authoring");
// </Dependencies>

// <Variables>
const key = 'REPLACE-WITH-YOUR-ASSIGNED-AUTHORING-KEY';

const endpoint = "https://REPLACE-WITH-RESOURCE-NAME.cognitiveservices.azure.cn/"
// </Variables>

// <AuthoringCreateClient>
const luisAuthoringCredentials = new msRest.ApiKeyCredentials({
  inHeader: { "Ocp-Apim-Subscription-Key": key }
});
const luisAuthoringClient = new LUIS.LUISAuthoringClient(
  luisAuthoringCredentials,
  endpoint
);
// </AuthoringCreateClient>

// <utilForQuickstartOnlyToSimulateAsyncOperation >
const delayTimer = async timeInMs => {
  return await new Promise(resolve => {
    setTimeout(resolve, timeInMs);
  });
};
// </utilForQuickstartOnlyToSimulateAsyncOperation >

// <AuthoringCreateApplication>
const create_app = async () => {
  const create_app_payload = {
    name: "Contoso",
    description: "Flight booking app built with Azure SDK for Java.",
    initialVersionId: "0.1",
    culture: "en-us"
  };

  const createAppResult = await luisAuthoringClient.apps.add(
    create_app_payload
  );

  console.log(`Created LUIS app with ID ${createAppResult.body}`);

  return {
    id: createAppResult.body,
    version: create_app_payload.initialVersionId
  };
}
// </AuthoringCreateApplication>

// <AuthoringAddEntities>
const add_entities = async app_info => {
  const addEntityDestinationResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Destination" }
  );
  console.log("Entity Destination created.");

  const addEntityClassResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Class" }
  );
  console.log("Entity Class created.");

  const addEntityFlightResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Flight" }
  );
  console.log("Entity Flight created.");
}
// </AuthoringAddEntities>

// <AuthoringAddIntents>
const add_intents = async app_info => {
  const addIntentFindFlightsResult = await luisAuthoringClient.model.addIntent(
    app_info.id,
    app_info.version,
    { name: "FindFlights" }
  );
  console.log("Intent FindFlights added.");
};
// </AuthoringAddIntents>

// <AuthoringBatchAddUtterancesForIntent>
const create_utterance = (intent, text, labels) => {
  var entityLabels = new Array();

  labels.forEach((value, key, map) => {
    const start_index = text.toLowerCase().indexOf(value.toLowerCase());
    const end_index = start_index + value.length;

    if (start_index > -1) {
      entityLabels.push({
        entityName: key,
        startCharIndex: start_index,
        endCharIndex: end_index
      });
    }
  });

  console.log(`Created ${entityLabels.length} entity labels.`);

  return { text: text, entityLabels: entityLabels, intentName: intent };
}

const add_utterances = async app_info => {
  const utterance_1 = create_utterance(
    "FindFlights",
    "find flights in economy to Madrid",
    new Map([
      ["Flight", "economy to Madrid"],
      ["Destination", "Madrid"],
      ["Class", "economy"]
    ])
  );
  const utterance_2 = create_utterance(
    "FindFlights",
    "find flights to London in first class",
    new Map([
      ["Flight", "London in first class"],
      ["Destination", "London"],
      ["Class", "first"]
    ])
  );
  const utterance_3 = create_utterance(
    "FindFlights",
    "find flights from seattle to London in first class",
    new Map([
      ["Flight", "flights from seattle to London in first class"],
      ["Location", "London"],
      ["Location", "Seattle"],
      ["Class", "first"]
    ])
  );

  const luisExamplesBatchResult = await luisAuthoringClient.examples.batch(
    app_info.id,
    app_info.version,
    [utterance_1, utterance_2, utterance_3]
  );
  console.log("Example utterances added.");
}
// </AuthoringBatchAddUtterancesForIntent>

// <AuthoringWaitForOperation>
const wait_for_operation = async app_info => {
  let operationResult = null;
  let modelUniqueStatus = ["InProgress"];

  while (
    modelUniqueStatus.includes("InProgress") ||
    modelUniqueStatus.includes("Queued")
  ) {
    await delayTimer(1000);

    operationResult = await luisAuthoringClient.train.getStatus(
      app_info.id,
      app_info.version
    );

    modelUniqueStatus = [
      ...new Set(operationResult.map(op => op.details.status))
    ];

    console.log(`Current model status: ${JSON.stringify(modelUniqueStatus)}`);
  }

  return operationResult;
}
// </AuthoringWaitForOperation>

// <AuthoringTrainVersion>
const train_app = async app_info => {
  const trainResult = await luisAuthoringClient.train.trainVersion(
    app_info.id,
    app_info.version
  );

  console.log("Waiting for train operation to finish...");

  const operationResult = await wait_for_operation(app_info);
}
// </AuthoringTrainVersion>

// <AuthoringPublishVersion>
const publish_app = async app_info => {
  const publishResult = await luisAuthoringClient.apps.publish(app_info.id, {
    versionId: app_info.version,
    isStaging: true
  });

  console.log(
    `Application published. Endpoint URL: ${publishResult.endpointUrl}`
  );
}
// </AuthoringPublishVersion>

// <AuthoringDeleteApp>
const delete_app = async app_info => {
  const deleteResult = await luisAuthoringClient.apps.deleteMethod(app_info.id);

  console.log(
    `Application with ID ${app_info.id} deleted. Operation result: ${deleteResult.message}`
  );
}
// </AuthoringDeleteApp>

// <AuthoringListApps>
const list_apps = async () => {
  const apps = await luisAuthoringClient.apps.list;

  for (let app of apps) {
    console.log(`ID ${app.id}, NAME ${app.name}`);
  }
}
// </AuthoringListApps>

// <Main>
const quickstart = async () => {
  //    list_apps();
  var app_info = await create_app();
  await add_entities(app_info);
  await add_intents(app_info);
  await add_utterances(app_info);
  await train_app(app_info);
  await publish_app(app_info);
  await delete_app(app_info);
}

try {
  quickstart();
} catch (error) {
  console.log(error);
}
// </Main>
```

## <a name="create-entities-for-the-app"></a>为应用创建实体

尽管实体不是必需的，但在大多数应用中都可以看到实体。 实体从用户言语中提取信息，只有使用这些信息才能实现用户的意向。 有多种类型的[预生成](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest#addcustomprebuiltentity-string--string--prebuiltdomainmodelcreateobject--msrest-requestoptionsbase-)实体和自定义实体，每种实体具有自身的数据转换对象 (DTO) 模型。  在应用中添加的常见预生成实体包括 [number](../luis-reference-prebuilt-number.md)、[datetimeV2](../luis-reference-prebuilt-datetimev2.md)、[geographyV2](../luis-reference-prebuilt-geographyv2.md) 和 [ordinal](../luis-reference-prebuilt-ordinal.md)。

此 **add_entities** 方法创建一个包含两个角色的 `Location` 简单实体、一个 `Class` 简单实体和一个 `Flight` 复合实体，并添加多个预生成实体。

必须知道，实体不会使用意向进行标记。 它们可以并且通常应用到多个意向。 只会为特定的单个意向标记示例用户言语。

实体的创建方法属于 [Model](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest) 类的一部分。 每个实体类型有自身的数据转换对象 (DTO) 模型。

```javascript
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Create an application.
 * - Add intents to an application.
 * - Add entities to an application.
 * - Add utterances to an application.
 * - Train an application.
 * - Publish an application.
 * - Delete an application.
 * - List all applications.
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-authoring
 */
// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-authoring");
// </Dependencies>

// <Variables>
const key = 'REPLACE-WITH-YOUR-ASSIGNED-AUTHORING-KEY';

const endpoint = "https://REPLACE-WITH-RESOURCE-NAME.cognitiveservices.azure.cn/"
// </Variables>

// <AuthoringCreateClient>
const luisAuthoringCredentials = new msRest.ApiKeyCredentials({
  inHeader: { "Ocp-Apim-Subscription-Key": key }
});
const luisAuthoringClient = new LUIS.LUISAuthoringClient(
  luisAuthoringCredentials,
  endpoint
);
// </AuthoringCreateClient>

// <utilForQuickstartOnlyToSimulateAsyncOperation >
const delayTimer = async timeInMs => {
  return await new Promise(resolve => {
    setTimeout(resolve, timeInMs);
  });
};
// </utilForQuickstartOnlyToSimulateAsyncOperation >

// <AuthoringCreateApplication>
const create_app = async () => {
  const create_app_payload = {
    name: "Contoso",
    description: "Flight booking app built with Azure SDK for Java.",
    initialVersionId: "0.1",
    culture: "en-us"
  };

  const createAppResult = await luisAuthoringClient.apps.add(
    create_app_payload
  );

  console.log(`Created LUIS app with ID ${createAppResult.body}`);

  return {
    id: createAppResult.body,
    version: create_app_payload.initialVersionId
  };
}
// </AuthoringCreateApplication>

// <AuthoringAddEntities>
const add_entities = async app_info => {
  const addEntityDestinationResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Destination" }
  );
  console.log("Entity Destination created.");

  const addEntityClassResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Class" }
  );
  console.log("Entity Class created.");

  const addEntityFlightResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Flight" }
  );
  console.log("Entity Flight created.");
}
// </AuthoringAddEntities>

// <AuthoringAddIntents>
const add_intents = async app_info => {
  const addIntentFindFlightsResult = await luisAuthoringClient.model.addIntent(
    app_info.id,
    app_info.version,
    { name: "FindFlights" }
  );
  console.log("Intent FindFlights added.");
};
// </AuthoringAddIntents>

// <AuthoringBatchAddUtterancesForIntent>
const create_utterance = (intent, text, labels) => {
  var entityLabels = new Array();

  labels.forEach((value, key, map) => {
    const start_index = text.toLowerCase().indexOf(value.toLowerCase());
    const end_index = start_index + value.length;

    if (start_index > -1) {
      entityLabels.push({
        entityName: key,
        startCharIndex: start_index,
        endCharIndex: end_index
      });
    }
  });

  console.log(`Created ${entityLabels.length} entity labels.`);

  return { text: text, entityLabels: entityLabels, intentName: intent };
}

const add_utterances = async app_info => {
  const utterance_1 = create_utterance(
    "FindFlights",
    "find flights in economy to Madrid",
    new Map([
      ["Flight", "economy to Madrid"],
      ["Destination", "Madrid"],
      ["Class", "economy"]
    ])
  );
  const utterance_2 = create_utterance(
    "FindFlights",
    "find flights to London in first class",
    new Map([
      ["Flight", "London in first class"],
      ["Destination", "London"],
      ["Class", "first"]
    ])
  );
  const utterance_3 = create_utterance(
    "FindFlights",
    "find flights from seattle to London in first class",
    new Map([
      ["Flight", "flights from seattle to London in first class"],
      ["Location", "London"],
      ["Location", "Seattle"],
      ["Class", "first"]
    ])
  );

  const luisExamplesBatchResult = await luisAuthoringClient.examples.batch(
    app_info.id,
    app_info.version,
    [utterance_1, utterance_2, utterance_3]
  );
  console.log("Example utterances added.");
}
// </AuthoringBatchAddUtterancesForIntent>

// <AuthoringWaitForOperation>
const wait_for_operation = async app_info => {
  let operationResult = null;
  let modelUniqueStatus = ["InProgress"];

  while (
    modelUniqueStatus.includes("InProgress") ||
    modelUniqueStatus.includes("Queued")
  ) {
    await delayTimer(1000);

    operationResult = await luisAuthoringClient.train.getStatus(
      app_info.id,
      app_info.version
    );

    modelUniqueStatus = [
      ...new Set(operationResult.map(op => op.details.status))
    ];

    console.log(`Current model status: ${JSON.stringify(modelUniqueStatus)}`);
  }

  return operationResult;
}
// </AuthoringWaitForOperation>

// <AuthoringTrainVersion>
const train_app = async app_info => {
  const trainResult = await luisAuthoringClient.train.trainVersion(
    app_info.id,
    app_info.version
  );

  console.log("Waiting for train operation to finish...");

  const operationResult = await wait_for_operation(app_info);
}
// </AuthoringTrainVersion>

// <AuthoringPublishVersion>
const publish_app = async app_info => {
  const publishResult = await luisAuthoringClient.apps.publish(app_info.id, {
    versionId: app_info.version,
    isStaging: true
  });

  console.log(
    `Application published. Endpoint URL: ${publishResult.endpointUrl}`
  );
}
// </AuthoringPublishVersion>

// <AuthoringDeleteApp>
const delete_app = async app_info => {
  const deleteResult = await luisAuthoringClient.apps.deleteMethod(app_info.id);

  console.log(
    `Application with ID ${app_info.id} deleted. Operation result: ${deleteResult.message}`
  );
}
// </AuthoringDeleteApp>

// <AuthoringListApps>
const list_apps = async () => {
  const apps = await luisAuthoringClient.apps.list;

  for (let app of apps) {
    console.log(`ID ${app.id}, NAME ${app.name}`);
  }
}
// </AuthoringListApps>

// <Main>
const quickstart = async () => {
  //    list_apps();
  var app_info = await create_app();
  await add_entities(app_info);
  await add_intents(app_info);
  await add_utterances(app_info);
  await train_app(app_info);
  await publish_app(app_info);
  await delete_app(app_info);
}

try {
  quickstart();
} catch (error) {
  console.log(error);
}
// </Main>
```

## <a name="add-example-utterance-to-intent"></a>将示例言语添加到意向

为了确定言语的意向并提取实体，应用需要言语示例。 这些示例需要针对特定的单个意向，并且应该标记所有自定义实体。 无需标记预生成实体。

通过创建 [ExampleLabelObject](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examplelabelobject?view=azure-node-latest) 对象的列表来添加示例言语（每个示例言语对应于一个对象）。 每个示例应使用实体名称和实体值的名称/值对字典来标记所有实体。 实体值应与示例言语文本中显示的值完全相同。

结合应用 ID、版本 ID 和示例列表调用 [examples.batch](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examples?view=azure-node-latest#batch-string--string--examplelabelobject----msrest-requestoptionsbase-)。 该调用将以结果列表做出响应。 需要检查每个示例的结果，以确保该示例已成功添加到模型中。

```javascript
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Create an application.
 * - Add intents to an application.
 * - Add entities to an application.
 * - Add utterances to an application.
 * - Train an application.
 * - Publish an application.
 * - Delete an application.
 * - List all applications.
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-authoring
 */
// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-authoring");
// </Dependencies>

// <Variables>
const key = 'REPLACE-WITH-YOUR-ASSIGNED-AUTHORING-KEY';

const endpoint = "https://REPLACE-WITH-RESOURCE-NAME.cognitiveservices.azure.cn/"
// </Variables>

// <AuthoringCreateClient>
const luisAuthoringCredentials = new msRest.ApiKeyCredentials({
  inHeader: { "Ocp-Apim-Subscription-Key": key }
});
const luisAuthoringClient = new LUIS.LUISAuthoringClient(
  luisAuthoringCredentials,
  endpoint
);
// </AuthoringCreateClient>

// <utilForQuickstartOnlyToSimulateAsyncOperation >
const delayTimer = async timeInMs => {
  return await new Promise(resolve => {
    setTimeout(resolve, timeInMs);
  });
};
// </utilForQuickstartOnlyToSimulateAsyncOperation >

// <AuthoringCreateApplication>
const create_app = async () => {
  const create_app_payload = {
    name: "Contoso",
    description: "Flight booking app built with Azure SDK for Java.",
    initialVersionId: "0.1",
    culture: "en-us"
  };

  const createAppResult = await luisAuthoringClient.apps.add(
    create_app_payload
  );

  console.log(`Created LUIS app with ID ${createAppResult.body}`);

  return {
    id: createAppResult.body,
    version: create_app_payload.initialVersionId
  };
}
// </AuthoringCreateApplication>

// <AuthoringAddEntities>
const add_entities = async app_info => {
  const addEntityDestinationResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Destination" }
  );
  console.log("Entity Destination created.");

  const addEntityClassResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Class" }
  );
  console.log("Entity Class created.");

  const addEntityFlightResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Flight" }
  );
  console.log("Entity Flight created.");
}
// </AuthoringAddEntities>

// <AuthoringAddIntents>
const add_intents = async app_info => {
  const addIntentFindFlightsResult = await luisAuthoringClient.model.addIntent(
    app_info.id,
    app_info.version,
    { name: "FindFlights" }
  );
  console.log("Intent FindFlights added.");
};
// </AuthoringAddIntents>

// <AuthoringBatchAddUtterancesForIntent>
const create_utterance = (intent, text, labels) => {
  var entityLabels = new Array();

  labels.forEach((value, key, map) => {
    const start_index = text.toLowerCase().indexOf(value.toLowerCase());
    const end_index = start_index + value.length;

    if (start_index > -1) {
      entityLabels.push({
        entityName: key,
        startCharIndex: start_index,
        endCharIndex: end_index
      });
    }
  });

  console.log(`Created ${entityLabels.length} entity labels.`);

  return { text: text, entityLabels: entityLabels, intentName: intent };
}

const add_utterances = async app_info => {
  const utterance_1 = create_utterance(
    "FindFlights",
    "find flights in economy to Madrid",
    new Map([
      ["Flight", "economy to Madrid"],
      ["Destination", "Madrid"],
      ["Class", "economy"]
    ])
  );
  const utterance_2 = create_utterance(
    "FindFlights",
    "find flights to London in first class",
    new Map([
      ["Flight", "London in first class"],
      ["Destination", "London"],
      ["Class", "first"]
    ])
  );
  const utterance_3 = create_utterance(
    "FindFlights",
    "find flights from seattle to London in first class",
    new Map([
      ["Flight", "flights from seattle to London in first class"],
      ["Location", "London"],
      ["Location", "Seattle"],
      ["Class", "first"]
    ])
  );

  const luisExamplesBatchResult = await luisAuthoringClient.examples.batch(
    app_info.id,
    app_info.version,
    [utterance_1, utterance_2, utterance_3]
  );
  console.log("Example utterances added.");
}
// </AuthoringBatchAddUtterancesForIntent>

// <AuthoringWaitForOperation>
const wait_for_operation = async app_info => {
  let operationResult = null;
  let modelUniqueStatus = ["InProgress"];

  while (
    modelUniqueStatus.includes("InProgress") ||
    modelUniqueStatus.includes("Queued")
  ) {
    await delayTimer(1000);

    operationResult = await luisAuthoringClient.train.getStatus(
      app_info.id,
      app_info.version
    );

    modelUniqueStatus = [
      ...new Set(operationResult.map(op => op.details.status))
    ];

    console.log(`Current model status: ${JSON.stringify(modelUniqueStatus)}`);
  }

  return operationResult;
}
// </AuthoringWaitForOperation>

// <AuthoringTrainVersion>
const train_app = async app_info => {
  const trainResult = await luisAuthoringClient.train.trainVersion(
    app_info.id,
    app_info.version
  );

  console.log("Waiting for train operation to finish...");

  const operationResult = await wait_for_operation(app_info);
}
// </AuthoringTrainVersion>

// <AuthoringPublishVersion>
const publish_app = async app_info => {
  const publishResult = await luisAuthoringClient.apps.publish(app_info.id, {
    versionId: app_info.version,
    isStaging: true
  });

  console.log(
    `Application published. Endpoint URL: ${publishResult.endpointUrl}`
  );
}
// </AuthoringPublishVersion>

// <AuthoringDeleteApp>
const delete_app = async app_info => {
  const deleteResult = await luisAuthoringClient.apps.deleteMethod(app_info.id);

  console.log(
    `Application with ID ${app_info.id} deleted. Operation result: ${deleteResult.message}`
  );
}
// </AuthoringDeleteApp>

// <AuthoringListApps>
const list_apps = async () => {
  const apps = await luisAuthoringClient.apps.list;

  for (let app of apps) {
    console.log(`ID ${app.id}, NAME ${app.name}`);
  }
}
// </AuthoringListApps>

// <Main>
const quickstart = async () => {
  //    list_apps();
  var app_info = await create_app();
  await add_entities(app_info);
  await add_intents(app_info);
  await add_utterances(app_info);
  await train_app(app_info);
  await publish_app(app_info);
  await delete_app(app_info);
}

try {
  quickstart();
} catch (error) {
  console.log(error);
}
// </Main>
```

## <a name="train-the-app"></a>训练应用

创建模型后，需要为此模型版本训练 LUIS 应用。 训练后的模型可在[容器](../luis-container-howto.md)中使用，或者将其[发布](../luis-how-to-publish-app.md)到过渡槽或生产槽。

[train.trainVersion](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#trainversion-string--string--msrest-requestoptionsbase-) 方法需要应用 ID 和版本 ID。

极小的模型（如本快速入门中所示的模型）很快就能完成训练。 对于生产级应用程序，应用的训练应该包括轮询调用 [get_status](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#getstatus-string--string--msrest-requestoptionsbase-) 方法以确定训练何时成功或者是否成功。 响应是一个 [ModelTrainingInfo](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/modeltraininginfo?view=azure-node-latest) 对象列表，其中分别列出了每个对象的状态。 所有对象必须成功，才能将训练视为完成。

```javascript
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Create an application.
 * - Add intents to an application.
 * - Add entities to an application.
 * - Add utterances to an application.
 * - Train an application.
 * - Publish an application.
 * - Delete an application.
 * - List all applications.
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-authoring
 */
// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-authoring");
// </Dependencies>

// <Variables>
const key = 'REPLACE-WITH-YOUR-ASSIGNED-AUTHORING-KEY';

const endpoint = "https://REPLACE-WITH-RESOURCE-NAME.cognitiveservices.azure.cn/"
// </Variables>

// <AuthoringCreateClient>
const luisAuthoringCredentials = new msRest.ApiKeyCredentials({
  inHeader: { "Ocp-Apim-Subscription-Key": key }
});
const luisAuthoringClient = new LUIS.LUISAuthoringClient(
  luisAuthoringCredentials,
  endpoint
);
// </AuthoringCreateClient>

// <utilForQuickstartOnlyToSimulateAsyncOperation >
const delayTimer = async timeInMs => {
  return await new Promise(resolve => {
    setTimeout(resolve, timeInMs);
  });
};
// </utilForQuickstartOnlyToSimulateAsyncOperation >

// <AuthoringCreateApplication>
const create_app = async () => {
  const create_app_payload = {
    name: "Contoso",
    description: "Flight booking app built with Azure SDK for Java.",
    initialVersionId: "0.1",
    culture: "en-us"
  };

  const createAppResult = await luisAuthoringClient.apps.add(
    create_app_payload
  );

  console.log(`Created LUIS app with ID ${createAppResult.body}`);

  return {
    id: createAppResult.body,
    version: create_app_payload.initialVersionId
  };
}
// </AuthoringCreateApplication>

// <AuthoringAddEntities>
const add_entities = async app_info => {
  const addEntityDestinationResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Destination" }
  );
  console.log("Entity Destination created.");

  const addEntityClassResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Class" }
  );
  console.log("Entity Class created.");

  const addEntityFlightResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Flight" }
  );
  console.log("Entity Flight created.");
}
// </AuthoringAddEntities>

// <AuthoringAddIntents>
const add_intents = async app_info => {
  const addIntentFindFlightsResult = await luisAuthoringClient.model.addIntent(
    app_info.id,
    app_info.version,
    { name: "FindFlights" }
  );
  console.log("Intent FindFlights added.");
};
// </AuthoringAddIntents>

// <AuthoringBatchAddUtterancesForIntent>
const create_utterance = (intent, text, labels) => {
  var entityLabels = new Array();

  labels.forEach((value, key, map) => {
    const start_index = text.toLowerCase().indexOf(value.toLowerCase());
    const end_index = start_index + value.length;

    if (start_index > -1) {
      entityLabels.push({
        entityName: key,
        startCharIndex: start_index,
        endCharIndex: end_index
      });
    }
  });

  console.log(`Created ${entityLabels.length} entity labels.`);

  return { text: text, entityLabels: entityLabels, intentName: intent };
}

const add_utterances = async app_info => {
  const utterance_1 = create_utterance(
    "FindFlights",
    "find flights in economy to Madrid",
    new Map([
      ["Flight", "economy to Madrid"],
      ["Destination", "Madrid"],
      ["Class", "economy"]
    ])
  );
  const utterance_2 = create_utterance(
    "FindFlights",
    "find flights to London in first class",
    new Map([
      ["Flight", "London in first class"],
      ["Destination", "London"],
      ["Class", "first"]
    ])
  );
  const utterance_3 = create_utterance(
    "FindFlights",
    "find flights from seattle to London in first class",
    new Map([
      ["Flight", "flights from seattle to London in first class"],
      ["Location", "London"],
      ["Location", "Seattle"],
      ["Class", "first"]
    ])
  );

  const luisExamplesBatchResult = await luisAuthoringClient.examples.batch(
    app_info.id,
    app_info.version,
    [utterance_1, utterance_2, utterance_3]
  );
  console.log("Example utterances added.");
}
// </AuthoringBatchAddUtterancesForIntent>

// <AuthoringWaitForOperation>
const wait_for_operation = async app_info => {
  let operationResult = null;
  let modelUniqueStatus = ["InProgress"];

  while (
    modelUniqueStatus.includes("InProgress") ||
    modelUniqueStatus.includes("Queued")
  ) {
    await delayTimer(1000);

    operationResult = await luisAuthoringClient.train.getStatus(
      app_info.id,
      app_info.version
    );

    modelUniqueStatus = [
      ...new Set(operationResult.map(op => op.details.status))
    ];

    console.log(`Current model status: ${JSON.stringify(modelUniqueStatus)}`);
  }

  return operationResult;
}
// </AuthoringWaitForOperation>

// <AuthoringTrainVersion>
const train_app = async app_info => {
  const trainResult = await luisAuthoringClient.train.trainVersion(
    app_info.id,
    app_info.version
  );

  console.log("Waiting for train operation to finish...");

  const operationResult = await wait_for_operation(app_info);
}
// </AuthoringTrainVersion>

// <AuthoringPublishVersion>
const publish_app = async app_info => {
  const publishResult = await luisAuthoringClient.apps.publish(app_info.id, {
    versionId: app_info.version,
    isStaging: true
  });

  console.log(
    `Application published. Endpoint URL: ${publishResult.endpointUrl}`
  );
}
// </AuthoringPublishVersion>

// <AuthoringDeleteApp>
const delete_app = async app_info => {
  const deleteResult = await luisAuthoringClient.apps.deleteMethod(app_info.id);

  console.log(
    `Application with ID ${app_info.id} deleted. Operation result: ${deleteResult.message}`
  );
}
// </AuthoringDeleteApp>

// <AuthoringListApps>
const list_apps = async () => {
  const apps = await luisAuthoringClient.apps.list;

  for (let app of apps) {
    console.log(`ID ${app.id}, NAME ${app.name}`);
  }
}
// </AuthoringListApps>

// <Main>
const quickstart = async () => {
  //    list_apps();
  var app_info = await create_app();
  await add_entities(app_info);
  await add_intents(app_info);
  await add_utterances(app_info);
  await train_app(app_info);
  await publish_app(app_info);
  await delete_app(app_info);
}

try {
  quickstart();
} catch (error) {
  console.log(error);
}
// </Main>
```

训练所有模型会花费一些时间。 使用 operationResult 检查训练状态。

```javascript
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Create an application.
 * - Add intents to an application.
 * - Add entities to an application.
 * - Add utterances to an application.
 * - Train an application.
 * - Publish an application.
 * - Delete an application.
 * - List all applications.
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-authoring
 */
// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-authoring");
// </Dependencies>

// <Variables>
const key = 'REPLACE-WITH-YOUR-ASSIGNED-AUTHORING-KEY';

const endpoint = "https://REPLACE-WITH-RESOURCE-NAME.cognitiveservices.azure.cn/"
// </Variables>

// <AuthoringCreateClient>
const luisAuthoringCredentials = new msRest.ApiKeyCredentials({
  inHeader: { "Ocp-Apim-Subscription-Key": key }
});
const luisAuthoringClient = new LUIS.LUISAuthoringClient(
  luisAuthoringCredentials,
  endpoint
);
// </AuthoringCreateClient>

// <utilForQuickstartOnlyToSimulateAsyncOperation >
const delayTimer = async timeInMs => {
  return await new Promise(resolve => {
    setTimeout(resolve, timeInMs);
  });
};
// </utilForQuickstartOnlyToSimulateAsyncOperation >

// <AuthoringCreateApplication>
const create_app = async () => {
  const create_app_payload = {
    name: "Contoso",
    description: "Flight booking app built with Azure SDK for Java.",
    initialVersionId: "0.1",
    culture: "en-us"
  };

  const createAppResult = await luisAuthoringClient.apps.add(
    create_app_payload
  );

  console.log(`Created LUIS app with ID ${createAppResult.body}`);

  return {
    id: createAppResult.body,
    version: create_app_payload.initialVersionId
  };
}
// </AuthoringCreateApplication>

// <AuthoringAddEntities>
const add_entities = async app_info => {
  const addEntityDestinationResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Destination" }
  );
  console.log("Entity Destination created.");

  const addEntityClassResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Class" }
  );
  console.log("Entity Class created.");

  const addEntityFlightResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Flight" }
  );
  console.log("Entity Flight created.");
}
// </AuthoringAddEntities>

// <AuthoringAddIntents>
const add_intents = async app_info => {
  const addIntentFindFlightsResult = await luisAuthoringClient.model.addIntent(
    app_info.id,
    app_info.version,
    { name: "FindFlights" }
  );
  console.log("Intent FindFlights added.");
};
// </AuthoringAddIntents>

// <AuthoringBatchAddUtterancesForIntent>
const create_utterance = (intent, text, labels) => {
  var entityLabels = new Array();

  labels.forEach((value, key, map) => {
    const start_index = text.toLowerCase().indexOf(value.toLowerCase());
    const end_index = start_index + value.length;

    if (start_index > -1) {
      entityLabels.push({
        entityName: key,
        startCharIndex: start_index,
        endCharIndex: end_index
      });
    }
  });

  console.log(`Created ${entityLabels.length} entity labels.`);

  return { text: text, entityLabels: entityLabels, intentName: intent };
}

const add_utterances = async app_info => {
  const utterance_1 = create_utterance(
    "FindFlights",
    "find flights in economy to Madrid",
    new Map([
      ["Flight", "economy to Madrid"],
      ["Destination", "Madrid"],
      ["Class", "economy"]
    ])
  );
  const utterance_2 = create_utterance(
    "FindFlights",
    "find flights to London in first class",
    new Map([
      ["Flight", "London in first class"],
      ["Destination", "London"],
      ["Class", "first"]
    ])
  );
  const utterance_3 = create_utterance(
    "FindFlights",
    "find flights from seattle to London in first class",
    new Map([
      ["Flight", "flights from seattle to London in first class"],
      ["Location", "London"],
      ["Location", "Seattle"],
      ["Class", "first"]
    ])
  );

  const luisExamplesBatchResult = await luisAuthoringClient.examples.batch(
    app_info.id,
    app_info.version,
    [utterance_1, utterance_2, utterance_3]
  );
  console.log("Example utterances added.");
}
// </AuthoringBatchAddUtterancesForIntent>

// <AuthoringWaitForOperation>
const wait_for_operation = async app_info => {
  let operationResult = null;
  let modelUniqueStatus = ["InProgress"];

  while (
    modelUniqueStatus.includes("InProgress") ||
    modelUniqueStatus.includes("Queued")
  ) {
    await delayTimer(1000);

    operationResult = await luisAuthoringClient.train.getStatus(
      app_info.id,
      app_info.version
    );

    modelUniqueStatus = [
      ...new Set(operationResult.map(op => op.details.status))
    ];

    console.log(`Current model status: ${JSON.stringify(modelUniqueStatus)}`);
  }

  return operationResult;
}
// </AuthoringWaitForOperation>

// <AuthoringTrainVersion>
const train_app = async app_info => {
  const trainResult = await luisAuthoringClient.train.trainVersion(
    app_info.id,
    app_info.version
  );

  console.log("Waiting for train operation to finish...");

  const operationResult = await wait_for_operation(app_info);
}
// </AuthoringTrainVersion>

// <AuthoringPublishVersion>
const publish_app = async app_info => {
  const publishResult = await luisAuthoringClient.apps.publish(app_info.id, {
    versionId: app_info.version,
    isStaging: true
  });

  console.log(
    `Application published. Endpoint URL: ${publishResult.endpointUrl}`
  );
}
// </AuthoringPublishVersion>

// <AuthoringDeleteApp>
const delete_app = async app_info => {
  const deleteResult = await luisAuthoringClient.apps.deleteMethod(app_info.id);

  console.log(
    `Application with ID ${app_info.id} deleted. Operation result: ${deleteResult.message}`
  );
}
// </AuthoringDeleteApp>

// <AuthoringListApps>
const list_apps = async () => {
  const apps = await luisAuthoringClient.apps.list;

  for (let app of apps) {
    console.log(`ID ${app.id}, NAME ${app.name}`);
  }
}
// </AuthoringListApps>

// <Main>
const quickstart = async () => {
  //    list_apps();
  var app_info = await create_app();
  await add_entities(app_info);
  await add_intents(app_info);
  await add_utterances(app_info);
  await train_app(app_info);
  await publish_app(app_info);
  await delete_app(app_info);
}

try {
  quickstart();
} catch (error) {
  console.log(error);
}
// </Main>
```

## <a name="publish-a-language-understanding-app"></a>发布语言理解应用

使用 [app.publish](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#publish-string--applicationpublishobject--msrest-requestoptionsbase-) 方法发布 LUIS 应用。 这会将当前已训练的版本发布到终结点上的指定槽。 客户端应用程序使用此终结点发送用户言语，以预测意向和提取实体。

```javascript
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Create an application.
 * - Add intents to an application.
 * - Add entities to an application.
 * - Add utterances to an application.
 * - Train an application.
 * - Publish an application.
 * - Delete an application.
 * - List all applications.
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-authoring
 */
// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-authoring");
// </Dependencies>

// <Variables>
const key = 'REPLACE-WITH-YOUR-ASSIGNED-AUTHORING-KEY';

const endpoint = "https://REPLACE-WITH-RESOURCE-NAME.cognitiveservices.azure.cn/"
// </Variables>

// <AuthoringCreateClient>
const luisAuthoringCredentials = new msRest.ApiKeyCredentials({
  inHeader: { "Ocp-Apim-Subscription-Key": key }
});
const luisAuthoringClient = new LUIS.LUISAuthoringClient(
  luisAuthoringCredentials,
  endpoint
);
// </AuthoringCreateClient>

// <utilForQuickstartOnlyToSimulateAsyncOperation >
const delayTimer = async timeInMs => {
  return await new Promise(resolve => {
    setTimeout(resolve, timeInMs);
  });
};
// </utilForQuickstartOnlyToSimulateAsyncOperation >

// <AuthoringCreateApplication>
const create_app = async () => {
  const create_app_payload = {
    name: "Contoso",
    description: "Flight booking app built with Azure SDK for Java.",
    initialVersionId: "0.1",
    culture: "en-us"
  };

  const createAppResult = await luisAuthoringClient.apps.add(
    create_app_payload
  );

  console.log(`Created LUIS app with ID ${createAppResult.body}`);

  return {
    id: createAppResult.body,
    version: create_app_payload.initialVersionId
  };
}
// </AuthoringCreateApplication>

// <AuthoringAddEntities>
const add_entities = async app_info => {
  const addEntityDestinationResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Destination" }
  );
  console.log("Entity Destination created.");

  const addEntityClassResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Class" }
  );
  console.log("Entity Class created.");

  const addEntityFlightResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Flight" }
  );
  console.log("Entity Flight created.");
}
// </AuthoringAddEntities>

// <AuthoringAddIntents>
const add_intents = async app_info => {
  const addIntentFindFlightsResult = await luisAuthoringClient.model.addIntent(
    app_info.id,
    app_info.version,
    { name: "FindFlights" }
  );
  console.log("Intent FindFlights added.");
};
// </AuthoringAddIntents>

// <AuthoringBatchAddUtterancesForIntent>
const create_utterance = (intent, text, labels) => {
  var entityLabels = new Array();

  labels.forEach((value, key, map) => {
    const start_index = text.toLowerCase().indexOf(value.toLowerCase());
    const end_index = start_index + value.length;

    if (start_index > -1) {
      entityLabels.push({
        entityName: key,
        startCharIndex: start_index,
        endCharIndex: end_index
      });
    }
  });

  console.log(`Created ${entityLabels.length} entity labels.`);

  return { text: text, entityLabels: entityLabels, intentName: intent };
}

const add_utterances = async app_info => {
  const utterance_1 = create_utterance(
    "FindFlights",
    "find flights in economy to Madrid",
    new Map([
      ["Flight", "economy to Madrid"],
      ["Destination", "Madrid"],
      ["Class", "economy"]
    ])
  );
  const utterance_2 = create_utterance(
    "FindFlights",
    "find flights to London in first class",
    new Map([
      ["Flight", "London in first class"],
      ["Destination", "London"],
      ["Class", "first"]
    ])
  );
  const utterance_3 = create_utterance(
    "FindFlights",
    "find flights from seattle to London in first class",
    new Map([
      ["Flight", "flights from seattle to London in first class"],
      ["Location", "London"],
      ["Location", "Seattle"],
      ["Class", "first"]
    ])
  );

  const luisExamplesBatchResult = await luisAuthoringClient.examples.batch(
    app_info.id,
    app_info.version,
    [utterance_1, utterance_2, utterance_3]
  );
  console.log("Example utterances added.");
}
// </AuthoringBatchAddUtterancesForIntent>

// <AuthoringWaitForOperation>
const wait_for_operation = async app_info => {
  let operationResult = null;
  let modelUniqueStatus = ["InProgress"];

  while (
    modelUniqueStatus.includes("InProgress") ||
    modelUniqueStatus.includes("Queued")
  ) {
    await delayTimer(1000);

    operationResult = await luisAuthoringClient.train.getStatus(
      app_info.id,
      app_info.version
    );

    modelUniqueStatus = [
      ...new Set(operationResult.map(op => op.details.status))
    ];

    console.log(`Current model status: ${JSON.stringify(modelUniqueStatus)}`);
  }

  return operationResult;
}
// </AuthoringWaitForOperation>

// <AuthoringTrainVersion>
const train_app = async app_info => {
  const trainResult = await luisAuthoringClient.train.trainVersion(
    app_info.id,
    app_info.version
  );

  console.log("Waiting for train operation to finish...");

  const operationResult = await wait_for_operation(app_info);
}
// </AuthoringTrainVersion>

// <AuthoringPublishVersion>
const publish_app = async app_info => {
  const publishResult = await luisAuthoringClient.apps.publish(app_info.id, {
    versionId: app_info.version,
    isStaging: true
  });

  console.log(
    `Application published. Endpoint URL: ${publishResult.endpointUrl}`
  );
}
// </AuthoringPublishVersion>

// <AuthoringDeleteApp>
const delete_app = async app_info => {
  const deleteResult = await luisAuthoringClient.apps.deleteMethod(app_info.id);

  console.log(
    `Application with ID ${app_info.id} deleted. Operation result: ${deleteResult.message}`
  );
}
// </AuthoringDeleteApp>

// <AuthoringListApps>
const list_apps = async () => {
  const apps = await luisAuthoringClient.apps.list;

  for (let app of apps) {
    console.log(`ID ${app.id}, NAME ${app.name}`);
  }
}
// </AuthoringListApps>

// <Main>
const quickstart = async () => {
  //    list_apps();
  var app_info = await create_app();
  await add_entities(app_info);
  await add_intents(app_info);
  await add_utterances(app_info);
  await train_app(app_info);
  await publish_app(app_info);
  await delete_app(app_info);
}

try {
  quickstart();
} catch (error) {
  console.log(error);
}
// </Main>
```

## <a name="delete-a-language-understanding-app"></a>删除语言理解应用

使用 [app.deleteMethod](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#deletemethod-string--models-appsdeletemethodoptionalparams-) 方法删除 LUIS 应用。 这将删除当前应用。

```javascript
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Create an application.
 * - Add intents to an application.
 * - Add entities to an application.
 * - Add utterances to an application.
 * - Train an application.
 * - Publish an application.
 * - Delete an application.
 * - List all applications.
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-authoring
 */
// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-authoring");
// </Dependencies>

// <Variables>
const key = 'REPLACE-WITH-YOUR-ASSIGNED-AUTHORING-KEY';

const endpoint = "https://REPLACE-WITH-RESOURCE-NAME.cognitiveservices.azure.cn/"
// </Variables>

// <AuthoringCreateClient>
const luisAuthoringCredentials = new msRest.ApiKeyCredentials({
  inHeader: { "Ocp-Apim-Subscription-Key": key }
});
const luisAuthoringClient = new LUIS.LUISAuthoringClient(
  luisAuthoringCredentials,
  endpoint
);
// </AuthoringCreateClient>

// <utilForQuickstartOnlyToSimulateAsyncOperation >
const delayTimer = async timeInMs => {
  return await new Promise(resolve => {
    setTimeout(resolve, timeInMs);
  });
};
// </utilForQuickstartOnlyToSimulateAsyncOperation >

// <AuthoringCreateApplication>
const create_app = async () => {
  const create_app_payload = {
    name: "Contoso",
    description: "Flight booking app built with Azure SDK for Java.",
    initialVersionId: "0.1",
    culture: "en-us"
  };

  const createAppResult = await luisAuthoringClient.apps.add(
    create_app_payload
  );

  console.log(`Created LUIS app with ID ${createAppResult.body}`);

  return {
    id: createAppResult.body,
    version: create_app_payload.initialVersionId
  };
}
// </AuthoringCreateApplication>

// <AuthoringAddEntities>
const add_entities = async app_info => {
  const addEntityDestinationResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Destination" }
  );
  console.log("Entity Destination created.");

  const addEntityClassResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Class" }
  );
  console.log("Entity Class created.");

  const addEntityFlightResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Flight" }
  );
  console.log("Entity Flight created.");
}
// </AuthoringAddEntities>

// <AuthoringAddIntents>
const add_intents = async app_info => {
  const addIntentFindFlightsResult = await luisAuthoringClient.model.addIntent(
    app_info.id,
    app_info.version,
    { name: "FindFlights" }
  );
  console.log("Intent FindFlights added.");
};
// </AuthoringAddIntents>

// <AuthoringBatchAddUtterancesForIntent>
const create_utterance = (intent, text, labels) => {
  var entityLabels = new Array();

  labels.forEach((value, key, map) => {
    const start_index = text.toLowerCase().indexOf(value.toLowerCase());
    const end_index = start_index + value.length;

    if (start_index > -1) {
      entityLabels.push({
        entityName: key,
        startCharIndex: start_index,
        endCharIndex: end_index
      });
    }
  });

  console.log(`Created ${entityLabels.length} entity labels.`);

  return { text: text, entityLabels: entityLabels, intentName: intent };
}

const add_utterances = async app_info => {
  const utterance_1 = create_utterance(
    "FindFlights",
    "find flights in economy to Madrid",
    new Map([
      ["Flight", "economy to Madrid"],
      ["Destination", "Madrid"],
      ["Class", "economy"]
    ])
  );
  const utterance_2 = create_utterance(
    "FindFlights",
    "find flights to London in first class",
    new Map([
      ["Flight", "London in first class"],
      ["Destination", "London"],
      ["Class", "first"]
    ])
  );
  const utterance_3 = create_utterance(
    "FindFlights",
    "find flights from seattle to London in first class",
    new Map([
      ["Flight", "flights from seattle to London in first class"],
      ["Location", "London"],
      ["Location", "Seattle"],
      ["Class", "first"]
    ])
  );

  const luisExamplesBatchResult = await luisAuthoringClient.examples.batch(
    app_info.id,
    app_info.version,
    [utterance_1, utterance_2, utterance_3]
  );
  console.log("Example utterances added.");
}
// </AuthoringBatchAddUtterancesForIntent>

// <AuthoringWaitForOperation>
const wait_for_operation = async app_info => {
  let operationResult = null;
  let modelUniqueStatus = ["InProgress"];

  while (
    modelUniqueStatus.includes("InProgress") ||
    modelUniqueStatus.includes("Queued")
  ) {
    await delayTimer(1000);

    operationResult = await luisAuthoringClient.train.getStatus(
      app_info.id,
      app_info.version
    );

    modelUniqueStatus = [
      ...new Set(operationResult.map(op => op.details.status))
    ];

    console.log(`Current model status: ${JSON.stringify(modelUniqueStatus)}`);
  }

  return operationResult;
}
// </AuthoringWaitForOperation>

// <AuthoringTrainVersion>
const train_app = async app_info => {
  const trainResult = await luisAuthoringClient.train.trainVersion(
    app_info.id,
    app_info.version
  );

  console.log("Waiting for train operation to finish...");

  const operationResult = await wait_for_operation(app_info);
}
// </AuthoringTrainVersion>

// <AuthoringPublishVersion>
const publish_app = async app_info => {
  const publishResult = await luisAuthoringClient.apps.publish(app_info.id, {
    versionId: app_info.version,
    isStaging: true
  });

  console.log(
    `Application published. Endpoint URL: ${publishResult.endpointUrl}`
  );
}
// </AuthoringPublishVersion>

// <AuthoringDeleteApp>
const delete_app = async app_info => {
  const deleteResult = await luisAuthoringClient.apps.deleteMethod(app_info.id);

  console.log(
    `Application with ID ${app_info.id} deleted. Operation result: ${deleteResult.message}`
  );
}
// </AuthoringDeleteApp>

// <AuthoringListApps>
const list_apps = async () => {
  const apps = await luisAuthoringClient.apps.list;

  for (let app of apps) {
    console.log(`ID ${app.id}, NAME ${app.name}`);
  }
}
// </AuthoringListApps>

// <Main>
const quickstart = async () => {
  //    list_apps();
  var app_info = await create_app();
  await add_entities(app_info);
  await add_intents(app_info);
  await add_utterances(app_info);
  await train_app(app_info);
  await publish_app(app_info);
  await delete_app(app_info);
}

try {
  quickstart();
} catch (error) {
  console.log(error);
}
// </Main>
```

## <a name="list-language-understanding-apps"></a>列出语言理解应用

列出与语言理解密钥关联的应用列表

```javascript
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Create an application.
 * - Add intents to an application.
 * - Add entities to an application.
 * - Add utterances to an application.
 * - Train an application.
 * - Publish an application.
 * - Delete an application.
 * - List all applications.
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-authoring
 */
// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-authoring");
// </Dependencies>

// <Variables>
const key = 'REPLACE-WITH-YOUR-ASSIGNED-AUTHORING-KEY';

const endpoint = "https://REPLACE-WITH-RESOURCE-NAME.cognitiveservices.azure.cn/"
// </Variables>

// <AuthoringCreateClient>
const luisAuthoringCredentials = new msRest.ApiKeyCredentials({
  inHeader: { "Ocp-Apim-Subscription-Key": key }
});
const luisAuthoringClient = new LUIS.LUISAuthoringClient(
  luisAuthoringCredentials,
  endpoint
);
// </AuthoringCreateClient>

// <utilForQuickstartOnlyToSimulateAsyncOperation >
const delayTimer = async timeInMs => {
  return await new Promise(resolve => {
    setTimeout(resolve, timeInMs);
  });
};
// </utilForQuickstartOnlyToSimulateAsyncOperation >

// <AuthoringCreateApplication>
const create_app = async () => {
  const create_app_payload = {
    name: "Contoso",
    description: "Flight booking app built with Azure SDK for Java.",
    initialVersionId: "0.1",
    culture: "en-us"
  };

  const createAppResult = await luisAuthoringClient.apps.add(
    create_app_payload
  );

  console.log(`Created LUIS app with ID ${createAppResult.body}`);

  return {
    id: createAppResult.body,
    version: create_app_payload.initialVersionId
  };
}
// </AuthoringCreateApplication>

// <AuthoringAddEntities>
const add_entities = async app_info => {
  const addEntityDestinationResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Destination" }
  );
  console.log("Entity Destination created.");

  const addEntityClassResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Class" }
  );
  console.log("Entity Class created.");

  const addEntityFlightResult = await luisAuthoringClient.model.addEntity(
    app_info.id,
    app_info.version,
    { name: "Flight" }
  );
  console.log("Entity Flight created.");
}
// </AuthoringAddEntities>

// <AuthoringAddIntents>
const add_intents = async app_info => {
  const addIntentFindFlightsResult = await luisAuthoringClient.model.addIntent(
    app_info.id,
    app_info.version,
    { name: "FindFlights" }
  );
  console.log("Intent FindFlights added.");
};
// </AuthoringAddIntents>

// <AuthoringBatchAddUtterancesForIntent>
const create_utterance = (intent, text, labels) => {
  var entityLabels = new Array();

  labels.forEach((value, key, map) => {
    const start_index = text.toLowerCase().indexOf(value.toLowerCase());
    const end_index = start_index + value.length;

    if (start_index > -1) {
      entityLabels.push({
        entityName: key,
        startCharIndex: start_index,
        endCharIndex: end_index
      });
    }
  });

  console.log(`Created ${entityLabels.length} entity labels.`);

  return { text: text, entityLabels: entityLabels, intentName: intent };
}

const add_utterances = async app_info => {
  const utterance_1 = create_utterance(
    "FindFlights",
    "find flights in economy to Madrid",
    new Map([
      ["Flight", "economy to Madrid"],
      ["Destination", "Madrid"],
      ["Class", "economy"]
    ])
  );
  const utterance_2 = create_utterance(
    "FindFlights",
    "find flights to London in first class",
    new Map([
      ["Flight", "London in first class"],
      ["Destination", "London"],
      ["Class", "first"]
    ])
  );
  const utterance_3 = create_utterance(
    "FindFlights",
    "find flights from seattle to London in first class",
    new Map([
      ["Flight", "flights from seattle to London in first class"],
      ["Location", "London"],
      ["Location", "Seattle"],
      ["Class", "first"]
    ])
  );

  const luisExamplesBatchResult = await luisAuthoringClient.examples.batch(
    app_info.id,
    app_info.version,
    [utterance_1, utterance_2, utterance_3]
  );
  console.log("Example utterances added.");
}
// </AuthoringBatchAddUtterancesForIntent>

// <AuthoringWaitForOperation>
const wait_for_operation = async app_info => {
  let operationResult = null;
  let modelUniqueStatus = ["InProgress"];

  while (
    modelUniqueStatus.includes("InProgress") ||
    modelUniqueStatus.includes("Queued")
  ) {
    await delayTimer(1000);

    operationResult = await luisAuthoringClient.train.getStatus(
      app_info.id,
      app_info.version
    );

    modelUniqueStatus = [
      ...new Set(operationResult.map(op => op.details.status))
    ];

    console.log(`Current model status: ${JSON.stringify(modelUniqueStatus)}`);
  }

  return operationResult;
}
// </AuthoringWaitForOperation>

// <AuthoringTrainVersion>
const train_app = async app_info => {
  const trainResult = await luisAuthoringClient.train.trainVersion(
    app_info.id,
    app_info.version
  );

  console.log("Waiting for train operation to finish...");

  const operationResult = await wait_for_operation(app_info);
}
// </AuthoringTrainVersion>

// <AuthoringPublishVersion>
const publish_app = async app_info => {
  const publishResult = await luisAuthoringClient.apps.publish(app_info.id, {
    versionId: app_info.version,
    isStaging: true
  });

  console.log(
    `Application published. Endpoint URL: ${publishResult.endpointUrl}`
  );
}
// </AuthoringPublishVersion>

// <AuthoringDeleteApp>
const delete_app = async app_info => {
  const deleteResult = await luisAuthoringClient.apps.deleteMethod(app_info.id);

  console.log(
    `Application with ID ${app_info.id} deleted. Operation result: ${deleteResult.message}`
  );
}
// </AuthoringDeleteApp>

// <AuthoringListApps>
const list_apps = async () => {
  const apps = await luisAuthoringClient.apps.list;

  for (let app of apps) {
    console.log(`ID ${app.id}, NAME ${app.name}`);
  }
}
// </AuthoringListApps>

// <Main>
const quickstart = async () => {
  //    list_apps();
  var app_info = await create_app();
  await add_entities(app_info);
  await add_intents(app_info);
  await add_utterances(app_info);
  await train_app(app_info);
  await publish_app(app_info);
  await delete_app(app_info);
}

try {
  quickstart();
} catch (error) {
  console.log(error);
}
// </Main>
```

## <a name="run-the-application"></a>运行应用程序

在快速入门文件中使用 `node luis_authoring_quickstart.js` 命令运行应用程序。

```console
node luis_authoring_quickstart.js
```

应用程序的命令行输出为：

```console
Created LUIS app with ID e137a439-b3e0-4e16-a7a8-a9746e0715f7
Entity Destination created.
Entity Class created.
Entity Flight created.
Intent FindFlights added.
Created 3 entity labels.
Created 3 entity labels.
Created 3 entity labels.
Example utterances added.
Waiting for train operation to finish...
Current model status: ["Queued"]
Current model status: ["InProgress"]
Current model status: ["InProgress"]
Current model status: ["InProgress"]
Current model status: ["Success"]
Application published. Endpoint URL: https://api.cognitive.azure.cn/luis/v2.0/apps/e137a439-b3e0-4e16-a7a8-a9746e0715f7
Application with ID e137a439-b3e0-4e16-a7a8-a9746e0715f7 deleted. Operation result: Operation Successful
```

