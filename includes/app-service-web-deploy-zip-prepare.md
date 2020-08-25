---
title: include 文件
description: include 文件
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 01/14/2020
ms.date: 08/13/2020
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: d2f7d3b7c879d9fffabe8e8b7417797aed616e2f
ms.sourcegitcommit: 9d9795f8a5b50cd5ccc19d3a2773817836446912
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88246522"
---
## <a name="create-a-project-zip-file"></a>创建一个项目 zip 文件

>[!NOTE]
> 如果以 zip 文件的形式下载文件，请首先提取该文件。 例如，如果从 GitHub 下载了 zip 文件，则无法按原样部署该文件。 GitHub 可添加额外的嵌套目录，但这不适用于应用服务。 
>

在本地终端窗口中，导航到应用项目的根目录。 

此目录应包含 Web 应用的入口文件，例如 _index.html_、_index.php_ 和 _app.js_。 它还可能包含包管理文件，如 _project.json_、_composer.json_、_package.json_、_bower.json_ 和 _requirements.txt_。

除非你希望应用服务为你运行部署自动化，否则请运行所有生成任务（例如，`npm`、`bower`、`gulp`、`composer` 和 `pip`），并确保你拥有运行应用所需的所有文件。 如果想要[直接运行包](../articles/app-service/deploy-run-package.md)，则此步骤是必需的。

创建一个包含项目所有内容的 zip 文件。 对于 `dotnet` 项目，此文件夹是 `dotnet publish` 命令的输出文件夹。 以下命令使用您终端中的默认工具执行操作：

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

