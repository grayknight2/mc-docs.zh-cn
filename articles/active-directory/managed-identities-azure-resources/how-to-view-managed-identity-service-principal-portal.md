---
title: 在 Azure 门户中查看托管标识的服务主体 - Azure AD
description: 在 Azure 门户中查看托管标识的服务主体的分步说明。
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/24/2020
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6143b59528d8845b84e7ed57e2867dfb28b7a92
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80243059"
---
# <a name="view-the-service-principal-of-a-managed-identity-in-the-azure-portal"></a>在 Azure 门户中查看托管标识的服务主体

Azure 资源的托管标识在 Azure Active Directory 中为 Azure 服务提供了一个自动托管标识。 此标识可用于通过支持 Azure AD 身份验证的任何服务的身份验证，这样就无需在代码中插入凭据了。 

本文将介绍如何在 Azure 门户中查看托管标识的服务主体。

 > [!NOTE] 
 > 服务主体是企业应用程序。 

## <a name="prerequisites"></a>必备条件

- 如果不熟悉 Azure 资源的托管标识，请查阅[概述部分](overview.md)。
- 如果还没有 Azure 帐户，请注册[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。
- 在[虚拟机](/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity)或[应用程序](/app-service/overview-managed-identity#add-a-system-assigned-identity)上启用系统分配的标识。

## <a name="view-the-service-principal"></a>查看服务主体

该过程演示如何查看启用了系统分配标识的 VM 的服务主体（相同的步骤也适用于应用程序）。

1. 依次单击“Azure Active Directory”、“企业应用程序”   。
2. 在“应用程序类型”  下，选择“所有应用程序”  ，然后单击“应用”  。
3. 在搜索筛选器框中，键入已启用托管标识的 VM 或应用程序的名称，或从显示的列表中选择它。

   ![在门户中查看托管标识服务主体](./media/how-to-view-managed-identity-service-principal-portal/view-managed-identity-service-principal-portal.png)

## <a name="next-steps"></a>后续步骤

[Azure 资源的托管标识](/active-directory/managed-identities-azure-resources/overview)


