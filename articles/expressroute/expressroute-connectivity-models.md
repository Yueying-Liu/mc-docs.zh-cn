---
title: ExpressRoute 连接模型：通过网络服务提供商、交换和以太网提供商连接到 Microsoft Azure | Azure
description: 本文介绍客户的网络与 Microsoft Azure、Office 365 和 Dynamics 365 服务之间的不同连接模式。 客户可以使用 MPLS 提供程序、云交换和以太网提供商。
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: ''
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 09/18/2019
ms.date: 12/02/2019
ms.author: v-yiso
ms.openlocfilehash: 19754dae03b998a8a04d8625eb5aa5b7affe0e56
ms.sourcegitcommit: 753c74533aca0310dc7acb621cfff5b8993c1d20
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92211486"
---
# <a name="expressroute-connectivity-models"></a>ExpressRoute 连接模型
在本地网络和 Microsoft 云之间创建连接时，可以采用三种不同的方式：[CloudExchange 归置](#CloudExchange)、[点到点以太网连接](#Ethernet)、[任意位置之间的 (IPVPN) 连接](#IPVPN)。 连接服务提供商可以提供一个或多个连接模型。 可以咨询连接服务提供商以选择最合适的模型。
<br><br>

![ExpressRoute 连接模型示意图](./media/expressroute-connectivity-models/expressroute-connectivity-models-diagram.png)

## <a name="co-located-at-a-cloud-exchange"></a><a name="CloudExchange"></a>共置于云交换位置
如果所在的位置提供云交换设施，则可以订购虚拟交叉连接，以通过共同租用提供商的以太网交换连接到 Microsoft 云。 共同租用提供商可以在共置设施中的基础结构与 Microsoft 云之间提供第 2 层交叉连接或托管的第 3 层交叉连接。

## <a name="point-to-point-ethernet-connections"></a><a name="Ethernet"></a>点到点以太网连接
可以通过点到点以太网链路，将本地数据中心/办公室连接到 Microsoft 云。 点到点以太网提供商可以在站点与 Microsoft 云之间提供第 2 层连接或托管的第 3 层连接。

## <a name="any-to-any-ipvpn-networks"></a><a name="IPVPN"></a>任意位置之间的 (IPVPN) 网络
可以将 WAN 集成到 Microsoft 云。 IPVPN 提供商（通常为 MPLS VPN）可在分支机构与数据中心之间提供任意位置之间的连接。 Microsoft 云可与 WAN 互连，就如同它是其他任何一个分支机构。 WAN 提供商通常提供托管的第 3 层连接。 在上述所有连接模型中，ExpressRoute 功能与特性完全相同。 

## <a name="next-steps"></a>后续步骤
* 了解 ExpressRoute 连接和路由域。 请参阅 [ExpressRoute 线路和路由域](./expressroute-circuit-peerings.md)。
* 了解 ExpressRoute 功能。 请参阅 [ExpressRoute 技术概述](./expressroute-introduction.md)
* 查找服务提供商。 请参阅 [ExpressRoute 合作伙伴和对等位置](./expressroute-locations.md)。
* 确保符合所有先决条件。 请参阅 [ExpressRoute 先决条件](./expressroute-prerequisites.md)。
* 请参阅[路由](expressroute-routing.md)、[NAT](expressroute-nat.md) 的要求。
* 配置 ExpressRoute 连接。
  * [创建 ExpressRoute 线路](./expressroute-howto-circuit-portal-resource-manager.md)
  * [配置路由](./expressroute-howto-routing-portal-resource-manager.md)
  * [将 VNet 链接到 ExpressRoute 线路](./expressroute-howto-linkvnet-portal-resource-manager.md)