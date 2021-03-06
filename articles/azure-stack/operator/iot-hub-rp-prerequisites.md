---
title: 在 Azure Stack Hub 上安装 IoT 中心的先决条件
description: 在 Azure Stack Hub 上安装 IoT 中心资源提供程序之前，先了解所需的先决条件。
author: WenJason
ms.author: v-jay
ms.service: azure-stack
ms.topic: how-to
origin.date: 1/6/2020
ms.date: 11/09/2020
ms.openlocfilehash: 8422734024d9d1a55c7177331b1873f07e687c1a
ms.sourcegitcommit: f187b1a355e2efafea30bca70afce49a2460d0c7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93330775"
---
# <a name="prerequisites-for-installing-iot-hub-on-azure-stack-hub"></a>在 Azure Stack Hub 上安装 IoT 中心的先决条件

[!INCLUDE [preview-banner](../includes/iot-hub-preview.md)]

必须先完成以下先决条件，然后才能在 Azure Stack Hub 上安装 IoT 中心。 可能需要几天或几周的提前期才能完成所有步骤。

> [!IMPORTANT]
> 这些先决条件假定你至少已部署了 4 节点 Azure Stack Hub 集成系统（生成号 1.2005.6.53 或更高）。 Azure Stack Hub 开发工具包 (ASDK) 不支持 IoT 中心资源提供程序。

## <a name="common-prerequisites"></a>常见的先决条件

[!INCLUDE [Common RP prerequisites](../includes/resource-provider-prerequisites.md)]

## <a name="dependency-prerequisites"></a>依赖项先决条件

1. 从市场下载并[安装事件中心](event-hubs-rp-install.md)。 开始部署 IoT 中心之前，必须先部署事件中心。
2. 为了更快地下载和安装 IoT 中心，请在下载 IoT 中心包之前从市场下载以下依赖项。 否则，IoT 中心部署将尝试下载相关包：
    * 自定义脚本扩展
    * PowerShell 所需状态配置
    * 免费许可证：Windows Server 2016 上的 SQL Server 2016 SP2 Express
    * SQL IaaS 扩展
    * Azure Stack 附加产品 RP Windows Server
3. 在成功安装事件中心之后，请等待至少 10 分钟，然后再继续部署 IoT 中心。

## <a name="certificate-requirements"></a>证书要求

1. 采购事件中心的公钥基础结构 (PKI) TLS/SSL 证书。 使用者备用名称 (SAN) 必须遵循以下命名模式：`CN=*.mgmtiothub.<region>.<fqdn>`。

   可以指定“使用者名称”，但在处理证书时 IoT 中心不会使用它。 仅使用使用者备用名称。 有关详细要求的完整列表，请参阅 [PKI 证书要求](azure-stack-pki-certs.md)。

   ![iot 中心证书示例](media\iot-hub-rp-prerequisites\certificate.png)

2. 请务必查看[验证你的证书](azure-stack-validate-pki-certs.md)。 本文介绍如何准备和验证用于 IoT 中心资源提供程序的证书。 

## <a name="dns-configuration-requirements"></a>DNS 配置要求
 
为了在 Azure Stack Hub 上使 IoT 中心在网络上正常工作，网络管理员需要配置 DNS。 在 DNS 管理工具中找到 DNS 条件转发设置，并添加一个条件转发规则，以允许 `<region>.cloudapp.<externaldomainname>` 的流量。 例如，`chinaeast.cloudapp.contoso.com`。

## <a name="next-steps"></a>后续步骤

接下来，在[连接的 Azure Stack](iot-hub-rp-install.md) 上安装 IoT 中心资源提供程序。
