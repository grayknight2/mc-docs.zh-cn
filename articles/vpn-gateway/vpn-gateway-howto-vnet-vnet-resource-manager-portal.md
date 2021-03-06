---
title: 配置 VNet 到 VNet VPN 网关连接：Azure 门户
description: 使用 Resource Manager 和 Azure 门户创建 VNet 之间的 VPN 网关连接。
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: conceptual
origin.date: 03/05/2020
ms.date: 04/06/2020
ms.author: v-jay
ms.openlocfilehash: f149ec81c3eae0b59234b21474c400c7b18c283a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80634600"
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-by-using-the-azure-portal"></a>使用 Azure 门户配置 VNet 到 VNet VPN 网关连接

本文介绍如何使用 VNet 到 VNet 连接类型来连接虚拟网络 (VNet)。 虚拟网络可以位于不同区域中。

![v2v 示意图](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

本文中的步骤适用于 Azure 资源管理器部署模型，并使用 Azure 门户。 可使用以下文章中所述的选项，通过不同的部署工具或模型创建此配置：

> [!div class="op_single_selector"]
> * [Azure 门户](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure 门户（经典）](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [连接不同的部署模型 - Azure 门户](vpn-gateway-connect-different-deployment-models-portal.md)
> * [连接不同的部署模型 - PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

## <a name="about-connecting-vnets"></a>关于连接 VNet

以下部分介绍如何通过不同的方式连接虚拟网络。

### <a name="vnet-to-vnet"></a>VNet 到 VNet

配置 VNet 到 VNet 连接是连接 VNet 的简单方式。 使用 VNet 到 VNet 连接类型 (VNet2VNet) 将一个虚拟网络连接到另一个虚拟网络类似于与本地位置建立站点到站点 IPsec 连接。 这两种连接类型都使用 VPN 网关来提供使用 IPsec/IKE 的安全隧道，两者在通信时的运行方式相同。 但是，两者在本地网络网关的配置方式上有差别。 

创建 VNet 到 VNet 连接时，不会自动创建和填充本地网络网关地址空间。 如果更新一个 VNet 的地址空间，另一个 VNet 会自动路由到更新的地址空间。 与创建站点到站点连接相比，创建 VNet 到 VNet 连接通常速度更快且更容易。

### <a name="site-to-site-ipsec"></a>站点到站点 (IPsec)

如果使用复杂的网络配置，你可能偏向于使用[站点到站点连接](vpn-gateway-howto-site-to-site-resource-manager-portal.md)来连接 VNet。 遵循站点到站点 IPsec 步骤时，可以手动创建和配置本地网络网关。 每个 VNet 的本地网关都将其他 VNet 视为本地站点。 使用这些步骤可为本地网络网关指定其他地址空间用于路由流量。 如果 VNet 的地址空间发生更改，必须手动更新相应的本地网络网关。

### <a name="vnet-peering"></a>VNet 对等互连

也可以使用 VNet 对等互连来连接 VNet。 VNet 对等互连不使用 VPN 网关，并且具有不同的约束。 另外，[VNet 对等互连定价](https://www.azure.cn/pricing/details/virtual-network)的计算不同于 [VNet 到 VNet VPN 网关定价](https://www.azure.cn/pricing/details/vpn-gateway)的计算。 有关详细信息，请参阅 [VNet 对等互连](../virtual-network/virtual-network-peering-overview.md)。

## <a name="why-create-a-vnet-to-vnet-connection"></a>为何创建 VNet 到 VNet 连接？

你可能会出于以下原因而使用 VNet 到 VNet 连接来连接虚拟网络：

### <a name="cross-region-geo-redundancy-and-geo-presence"></a>跨区域异地冗余和地区存在

  * 可以使用安全连接设置自己的异地复制或同步，而无需借助于面向 Internet 的终结点。
  * 使用 Azure 流量管理器和 Azure 负载均衡器，可以设置支持跨多个 Azure 区域实现异地冗余的高可用性工作负荷。 例如，可跨多个 Azure 区域中设置 SQL Always On 可用性组。

### <a name="regional-multi-tier-applications-with-isolation-or-administrative-boundaries"></a>具有隔离或管理边界的区域多层应用程序

  * 在同一区域中，由于存在隔离或管理要求，可以设置多个虚拟网络连接在一起的多层应用程序。

可以将 VNet 到 VNet 通信与多站点配置组合使用。 使用这些配置可以建立将跨界连接与虚拟网络间连接相结合的网络拓扑，如下图所示：

![关于连接](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "关于连接")

本文介绍如何使用 VNet 到 VNet 连接类型来连接 VNet。 遵循这些步骤进行练习时，可以使用以下示例设置值。 示例中的虚拟网络在同一订阅中，但却在不同的资源组中。 有关 VNet 到 VNet 连接的详细信息，请参阅 [VNet 到 VNet 连接常见问题解答](#vnet-to-vnet-faq)。

### <a name="example-settings"></a>示例设置

**VNet1 的值：**

- **虚拟网络设置**
    - **名称**：VNet1
    - **地址空间**：10.1.0.0/16
    - **订阅**：选择要使用的订阅。
    - **资源组**：TestRG1
    - **位置**：中国北部
    - **子网**
        - **名称**：FrontEnd
        - **地址范围**：10.1.0.0/24
    - **网关子网**：
        - **名称**：“GatewaySubnet”会自动填充 
        - **地址范围**：10.1.255.0/27

- **虚拟网络网关设置**
    - **名称**：VNet1GW
    - **网关类型**：选择“VPN”。 
    - **VPN 类型**：选择“基于路由”。 
    - **SKU**：选择要使用的网关 SKU。
    - **公共 IP 地址名称**：VNet1GWpip
    - **Connection**
       - **名称**：VNet1 到 VNet4
       - **共享密钥**：可以自行创建共享密钥。 在 VNet 之间建立连接时，上述值必须匹配。 对于此练习，请使用 abc123。

**VNet4 的值：**

- **虚拟网络设置**
   - **名称**：VNet4
   - **地址空间**：10.41.0.0/16
   - **订阅**：选择要使用的订阅。
   - **资源组**：TestRG4
   - **位置**：中国北部
   - **子网** 
      - **名称**：FrontEnd
      - **地址范围**：10.41.0.0/24
   - **GatewaySubnet** 
      - **名称**：“GatewaySubnet”会自动填充 
      - **地址范围**：10.41.255.0/27

- **虚拟网络网关设置** 
    - **名称**：VNet4GW
    - **网关类型**：选择“VPN”。 
    - **VPN 类型**：选择“基于路由”。 
    - **SKU**：选择要使用的网关 SKU。
    - **公共 IP 地址名称**：VNet4GWpip
    - **Connection** 
       - **名称**：VNet4 到 VNet1
       - **共享密钥**：可以自行创建共享密钥。 在 VNet 之间建立连接时，上述值必须匹配。 对于此练习，请使用 abc123。

## <a name="create-and-configure-vnet1"></a>创建并配置 VNet1
如果已有一个 VNet，请检查其设置是否与 VPN 网关设计兼容。 请特别注意任何可能与其他网络重叠的子网。 如果有重叠的子网，将无法正常连接。

### <a name="to-create-a-virtual-network"></a>创建虚拟网络
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="create-the-vnet1-gateway"></a>创建 VNet1 网关
在此步骤中为 VNet 创建虚拟网络网关。 创建网关通常需要 45 分钟或更长的时间，具体取决于所选网关 SKU。 如果你正在练习创建此配置，请参阅[示例设置](#example-settings)。

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

### <a name="to-create-a-virtual-network-gateway"></a>创建虚拟网络网关
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="create-and-configure-vnet4"></a>创建并配置 VNet4
配置 VNet1 后，请创建 VNet4 和 VNet4 网关，方法是：重复上述步骤并将值替换为 VNet4 值。 无需等到 VNet1 的虚拟网关创建完成即可配置 VNet4。 如果使用自己的值，请确保地址空间不与任何想要连接的 VNet 重叠。

## <a name="configure-the-vnet1-gateway-connection"></a>配置 VNet1 网关连接
VNet1 和 VNet4 的虚拟网关都已完成后，便可以创建虚拟网关连接。 在本部分，请创建从 VNet1 到 VNet4 的连接。 这些步骤仅适用于同一订阅中的 VNet。 不过，如果 VNet 位于同一订阅的不同资源组中，则可使用门户来连接它们。

1. 在 Azure 门户中选择“所有资源”，在搜索框中输入“虚拟网络网关”，然后导航到 VNet 的虚拟网络网关。   例如，**VNet1GW**。 选择网关，打开“虚拟网关”页。  在“设置”  下，选择“连接”  。

   ![“连接”页](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connections.png "“连接”页")
2. 选择“+添加”  ，打开“添加连接”  页。

   ![添加连接](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/vnet1-vnet4-connection.png "添加连接")
3. 在“添加连接”页上，填写用于建立连接的值： 

   - **名称**：输入连接的名称。 例如，*VNet1toVNet4*。

   - **连接类型**：从下拉列表选择“VNet 到 VNet”  。

   - **第一个虚拟网络网关**：此字段值会自动填充，因为要从指定的虚拟网络网关建立此连接。

   - **第二个虚拟网络网关**：此字段是要连接到的 VNet 的虚拟网络网关。 选择“选择另一个虚拟网络网关”打开“选择虚拟网络网关”页。  

     - 查看此页上列出的虚拟网关。 请注意，只会列出订阅中的虚拟网络网关。

     - 选择要连接的虚拟网络网关。

     - **共享密钥(PSK)** ：在此字段中，输入连接的共享密钥。 可以自己生成或创建此密钥。 在站点到站点连接中，使用的密钥与本地设备和虚拟网络网关连接的密钥相同。 此处的概念大致相同，不过，此时不是连接到 VPN 设备，而是连接到另一个虚拟网络网关。
    
4. 选择“确定”  保存更改。

## <a name="configure-the-vnet4-gateway-connection"></a>配置 VNet4 网关连接
接下来，创建一个从 VNet4 到 VNet1 的连接。 在门户中找到与 VNet4 关联的虚拟网关。 按上一部分的步骤替换相关值，创建从 VNet4 到 VNet1 的连接。 请确保使用相同的共享密钥。

## <a name="verify-your-connections"></a>验证连接

1. 在 Azure 门户中找到虚拟网络网关。 
2. 在“虚拟网络网关”页上选择“连接”，查看虚拟网络网关的“连接”页。    建立连接后，会看到“状态”值更改为“已连接”。  

   ![验证连接](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/view-connections.png "验证连接")
3. 在“名称”  列下选择一个连接，查看其详细信息。 数据开始流动后，会看到“输入数据”和“输出数据”的值。  

   ![状态](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/status.png "状态")

## <a name="add-additional-connections"></a>添加其他连接

若要添加其他连接，请导航到要从中创建连接的虚拟网络网关，然后选择“连接”。  可以创建另一个 VNet 到 VNet 连接，也可以创建一个 IPsec 站点到站点连接，以便连接到本地位置。 请务必调节“连接类型”  ，使之与要创建的连接类型匹配。 在创建其他连接之前，请验证虚拟网络的地址空间是否不与要连接到的地址空间重叠。 如需创建站点到站点连接的步骤，请参阅[创建站点到站点连接](vpn-gateway-howto-site-to-site-resource-manager-portal.md)。

## <a name="vnet-to-vnet-faq"></a>VNet 到 VNet 常见问题
查看常见问题解答详细信息以获取有关 VNet 到 VNet 连接的其他信息。

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]

## <a name="next-steps"></a>后续步骤

有关如何限制发往虚拟网络中资源的网络流量的信息，请参阅[网络安全性](../virtual-network/security-overview.md)。

有关 Azure 如何在 Azure 资源、本地资源和 Internet 资源之间路由流量的信息，请参阅[虚拟网络流量路由](../virtual-network/virtual-networks-udr-overview.md)。
