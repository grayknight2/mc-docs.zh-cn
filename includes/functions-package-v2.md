---
title: include 文件
description: include 文件
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 02/17/2020
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 11f3ac4cd572b482650ca206c1e65089a8b99637
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77494084"
---
使用以下方法在首选开发环境中添加支持。

| 开发环境  | 应用程序类型      | 添加支持 |
|--------------------------|-----------------------|----------------|
| Visual Studio            | C# 类库      | [安装 NuGet 包](../articles/azure-functions/functions-bindings-register.md#vs) |
| Visual Studio Code       | 基于[核心工具](../articles/azure-functions/functions-run-local.md) | [注册扩展捆绑包](../articles/azure-functions/functions-bindings-register.md#extension-bundles)<br><br>建议安装 [Azure Tools 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)。 |
| 任何其他编辑器/IDE     | 基于[核心工具](../articles/azure-functions/functions-run-local.md) | [注册扩展捆绑包](../articles/azure-functions/functions-bindings-register.md#extension-bundles) |
| Azure 门户             | 仅在门户中联机 | 在添加绑定时安装<br /><br /> 请参阅[更新扩展](../articles/azure-functions/install-update-binding-extensions-manual.md)以更新现有绑定扩展，而无需重新发布函数应用。 |

