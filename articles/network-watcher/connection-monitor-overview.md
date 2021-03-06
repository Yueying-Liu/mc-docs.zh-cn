---
title: 连接监视器（预览版）| Azure Docs
description: 了解如何使用连接监视器（预览版）监视分布式环境中的网络通信。
services: network-watcher
documentationcenter: na
manager: agummadi
editor: ''
tags: azure-resource-manager
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 01/27/2020
author: rockboyfor
ms.date: 11/30/2020
ms.testscope: yes
ms.testdate: 08/03/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 887d9d962e32b155d5da1abe1b4d11efc0432f07
ms.sourcegitcommit: abc264824023c7cb7abf74898fc1b375e730545f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96231338"
---
# <a name="network-connectivity-monitoring-with-connection-monitor-preview"></a>使用连接监视器（预览版）进行网络连接监视

连接监视器（预览版）在 Azure 网络观察程序中提供统一的端到端连接监视。 连接监视器（预览版）功能支持混合部署和 Azure 云部署。 网络观察程序提供的工具可用于监视、诊断和查看针对 Azure 部署的与连接相关的指标。

下面是连接监视器（预览版）的一些用例：

- 前端 Web 服务器 VM 与多层应用程序中的数据库服务器 VM 进行通信。 你希望检查两个 VM 之间的网络连接。
- 你希望中国东部区域中的 VM 能够端对端连接到中国北部区域中的 VM，并且希望比较两者的跨区域网络延迟情况。

    <!-Not Available on - You have multiple on-premises office sites in Seattle, Washington, and in Ashburn, Virginia. Your office sites connect to Office 365 URLs. For your users of Office 365 URLs, compare the latencies between Seattle and Ashburn.-->

- 混合应用程序需要连接到 Azure 存储终结点。 本地站点和 Azure 应用程序连接到相同的 Azure 存储终结点。 希望比较本地站点的延迟与 Azure 应用程序的延迟。
- 希望检查本地设置与托管云应用程序的 Azure VM 之间的连接。

在其预览阶段，连接监视器结合了后面两项功能的优点：网络观察程序[连接监视器](https://docs.azure.cn/network-watcher/network-watcher-monitoring-overview#monitor-communication-between-a-virtual-machine-and-an-endpoint)功能，以及网络性能监视器 (NPM) [服务连接监视器](https://docs.azure.cn/azure-monitor/insights/network-performance-monitor-service-connectivity)和[性能监视](https://docs.azure.cn/azure-monitor/insights/network-performance-monitor-performance-monitor)功能。

<!--Not Available on [ExpressRoute Monitoring](https://docs.azure.cn/expressroute/how-to-npm)-->

下面是连接监视器（预览版）的一些优点：

* 适用于 Azure 和混合监视需求的直观一致的体验
* 跨区域且跨工作区的连接监视
* 更高的探测频率，更好地了解网络性能
* 为混合部署提供更快的警报
* 支持基于 HTTP、TCP 和 ICMP 的连接检查 
* 同时适用于 Azure 和非 Azure 测试设置的指标和 Log Analytics 支持

:::image type="content" source="./media/connection-monitor-2-preview/hero-graphic.png" alt-text="显示连接监视器如何与 Azure VM、非 Azure 主机、终结点和数据存储位置进行交互的示意图":::

若要开始使用连接监视器（预览版）进行监视，请执行以下步骤： 

1. 安装监视代理。
1. 在订阅上启用网络观察程序。
1. 创建连接监视器。
1. 设置数据分析和警报。
1. 诊断网络中的问题。

以下部分提供了更多有关这些步骤的详细信息。

## <a name="install-monitoring-agents"></a>安装监视代理

连接监视器依赖轻型的可执行文件来运行连接性检查。  它同时支持来自 Azure 环境和本地环境的连接性检查。 使用的可执行文件取决于 VM 是托管在 Azure 上还是托管在本地。

### <a name="agents-for-azure-virtual-machines"></a>Azure 虚拟机代理

若要使连接监视器将 Azure VM 识别为监视源，请在其上安装网络观察程序代理虚拟机扩展。 此扩展也称为 *网络观察程序扩展*。 Azure 虚拟机需要该扩展来触发端对端监控和其他高级功能。 

可以在[创建 VM](https://docs.azure.cn/network-watcher/connection-monitor#create-the-first-vm) 时安装网络观察程序扩展。 还可以为 [Linux](https://docs.azure.cn/virtual-machines/extensions/network-watcher-linux) 和 [Windows](https://docs.azure.cn/virtual-machines/extensions/network-watcher-windows) 分别安装、配置网络观察程序扩展并对其进行故障排除。

用于网络安全组 (NSG) 或防火墙的规则可以阻止源和目标之间的通信。 连接监视器检测到此问题并将其显示为拓扑中的诊断消息。 若要启用连接监视，请确保 NSG 和防火墙规则允许在源和目标之间使用 TCP 或 ICMP 上的数据包。

### <a name="agents-for-on-premises-machines"></a>本地计算机的代理

若要使连接监视器将本地计算机识别为要监视的源，请在计算机上安装 Log Analytics 代理。 然后启用网络性能监视器解决方案。 这些代理是链接到 Log Analytics 工作区的，因此，需要先设置工作区 ID 和主密钥，然后代理才能开始进行监视。

若要为 Windows 计算机安装 Log Analytics 代理，请参阅[适用于 Windows 的 Azure Monitor 虚拟机扩展](https://docs.azure.cn/virtual-machines/extensions/oms-windows)。

如果路径包括防火墙或网络虚拟设备 (NVA)，请确保可访问目标。

## <a name="enable-network-watcher-on-your-subscription"></a>在订阅上启用网络观察程序

使用网络观察程序启用具有虚拟网络的所有订阅。 在订阅中创建虚拟网络时，虚拟网络的区域和订阅中会自动启用网络观察程序。 这种自动启用不会影响资源，也不会产生费用。 确保未在订阅上显式禁用网络观察程序。 

有关详细信息，请参阅[启用网络观察程序](https://docs.azure.cn/network-watcher/network-watcher-create)。

## <a name="create-a-connection-monitor"></a>创建连接监视器 

连接监视器会定期监视通信。 它会通知你有关可访问性和延迟的更改情况。 还可以检查源代理和目标终结点之间当前的和历史的网络拓扑。

源可以是 Azure VM，源也可以是具有已安装的监视代理的本地计算机。 目标终结点可以是 Microsoft 365 URL、Dynamics 365 URL、自定义 URL、Azure VM 资源 ID、IPv4、IPv6、FQDN 或任何域名。

### <a name="access-connection-monitor-preview"></a>访问连接监视器（预览版）

1. 在 Azure 门户主页上，转到“网络观察程序”。
1. 在左侧的“监视”部分，选择“连接监视器（预览版）” 。
1. 你会看到在连接监视器（预览版）中创建的所有连接监视器。 若要查看在经典连接监视器体验中创建的连接监视器，请转到“连接监视器”选项卡。

    :::image type="content" source="./media/connection-monitor-2-preview/cm-resource-view.png" alt-text="显示连接监视器（预览版）中创建的连接监视器的屏幕截图" lightbox="./media/connection-monitor-2-preview/cm-resource-view.png":::

### <a name="create-a-connection-monitor"></a>创建连接监视器

在连接监视器（预览版）中创建的连接监视器中，可以将本地计算机和 Azure VM 添加为源。 这些连接监视器还可以监视与终结点的连接。 终结点可以位于 Azure 上，也可以位于任何其他 URL 或 IP 上。

连接监视器（预览版）包含以下实体：

* **连接监视器资源** - 特定于区域的 Azure 资源。 以下所有实体都是连接监视器资源的属性。
* **终结点** - 参与连接检查的源或目标。 终结点的示例包括 Azure VM、本地代理、URL 和 IP。
* **测试配置** - 针对测试的特定于协议的配置。 根据选定协议，可以定义端口、阈值、测试频率和其他参数。
* **测试组** -包含源终结点、目标终结点和测试配置的组。 连接监视器可包含多个测试组。
* **测试** - 将源终结点、目标终结点和测试配置组合在一起。 测试是可用于监视数据的最精细级别。 监视数据包括检查失败的百分比和往返时间 (RTT)。

    :::image type="content" source="./media/connection-monitor-2-preview/cm-tg-2.png" alt-text="显示连接监视器的示意图，其中定义了测试组和测试之间的关系":::

可使用 [Azure 门户](connection-monitor-preview-create-using-portal.md)或 [ARMClient](connection-monitor-preview-create-using-arm-client.md) 创建连接监视器预览版

添加到测试组中的所有源、目标和测试配置将细分为单独的测试。 下面是如何分解源和目标的示例：

* 测试组：TG1
* 源：3 (A, B, C)
* 目标：2 (D, E)
* 测试配置：2 (Config 1, Config 2)
* 已创建的测试总数：12

| 测试编号 | Source | 目标 | 测试配置 |
| --- | --- | --- | --- |
| 1 | A | D | Config 1 |
| 2 | A | D | Config 2 |
| 3 | A | E | Config 1 |
| 4 | A | E | Config 2 |
| 5 | B | D | Config 1 |
| 6 | B | D | Config 2 |
| 7 | B | E | Config 1 |
| 8 | B | E | Config 2 |
| 9 | C | D | Config 1 |
| 10 | C | D | Config 2 |
| 11 | C | E | Config 1 |
| 12 | C | E | Config 2 |

### <a name="scale-limits"></a>规模限制

连接监视器具有以下规模限制：

* 每个区域每个订阅的最大连接监视器数：100
* 每个连接监视器的最大测试组：20 个
* 每个连接监视器的最大源和目标：100
* 每个连接监视器的最大测试组：20

## <a name="analyze-monitoring-data-and-set-alerts"></a>分析监视数据并设置警报

创建连接监视器后，源会根据测试配置来检查与目标的连接。

### <a name="checks-in-a-test"></a>签入测试

根据你在测试配置中所选的协议，连接监视器（预览版）对源-目标对运行一系列检查。 根据所选测试频率运行检查。

如果使用 HTTP，则服务会计算返回了有效响应代码的 HTTP 响应数。 可使用 PowerShell 和 CLI 设置有效的响应代码。 结果确定检查失败的百分比。 为了计算 RTT，服务会测量 HTTP 调用和响应之间的时间。

如果使用 TCP 或 ICMP，则服务会计算数据包丢失百分比，以确定检查失败的百分比。 若要计算 RTT，则服务会测量为发送的数据包接收确认 (ACK) 所花费的时间。 如果已为网络测试启用了跟踪路由数据，则可以查看本地网络的逐跳丢失和延迟。

### <a name="states-of-a-test"></a>测试的状态

根据检查返回的数据，测试可以具有以下状态：

* **传递** - 检查失败和 RTT 百分比的实际值都在指定的阈值内。
* **失败** - 检查失败或 RTT 百分比的实际值超出指定的阈值。 如果未指定阈值，则当检查失败的百分比为 100 时，测试进入失败状态。
* **警告** - 
     * 如果指定了阈值，并且连接监视器（预览版）观察到检查失败百分比超过阈值的 80%，则测试将标记为“警告”。
     * 如果没有指定阈值，连接监视器（预览版）会自动分配一个阈值。 超过此阈值时，测试状态将更改为“警告”。 对于 TCP 或 ICMP 测试中的往返时间，阈值为 750msec。 对于检查失败百分比，阈值为 10%。 
* **不确定** - Log Analytics 工作区中没有数据。  检查指标。 
* **未运行** - 已通过禁用测试组禁用  

### <a name="data-collection-analysis-and-alerts"></a>数据收集、分析和警报

连接监视器（预览版）收集的数据存储在 Log Analytics 工作区中。 创建连接监视器时设置此工作区。 

监视数据也可用于 Azure Monitor 指标中。 可以根据需要使用 Log Analytics 保留监视数据。 默认情况下，Azure Monitor 仅存储 30 天的指标。 

可以[根据数据设置基于指标的警报](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts/)。

#### <a name="monitoring-dashboards"></a>监视仪表板

在监视仪表板上，可以查看可以访问的订阅、区域、时间戳、源和目标类型的连接监视器列表。

从网络观察程序中转到连接监视器（预览版）时，可以通过以下方式查看数据：

* **连接监视器** - 为订阅、区域、时间戳、源和目标类型创建的所有连接监视器的列表。 此视图为默认视图。
* **测试组** - 为订阅、区域、时间戳、源和目标类型创建的所有测试组的列表。 这些测试组不由连接监视器进行筛选。
* **测试** - 运行订阅、区域、时间戳、源和目标类型的所有测试的列表。 这些测试不由连接监视器或测试组进行筛选。

在下图中，这三个数据视图由箭头 1 指示。

在仪表板上，可以展开每个连接监视器以查看其测试组。 然后，可以展开每个测试组，以查看在其中运行的测试。 

你可以基于以下内容筛选列表：

* **顶层筛选器** - 按文本、实体类型（连接监视器、测试组或测试）时间戳和范围搜索列表。 范围包括订阅、区域、源和目标类型。 请查看下图中的框 1。
* **基于状态的筛选** - 按连接监视器、测试组或测试的状态进行筛选。 请查看下列图像中的框 2。
* **基于警报的筛选器** - 按连接监视器资源上触发的警报进行筛选。 请查看下图中的框 3。

  :::image type="content" source="./media/connection-monitor-2-preview/cm-view.png" alt-text="显示如何筛选连接监视器（预览版）中的连接监视器、测试组和测试的视图的屏幕截图" lightbox="./media/connection-monitor-2-preview/cm-view.png":::

例如，若要查看其中源 IP 是 10.192.64.56 的连接监视器（预览版）中的所有测试，请执行以下操作：
1. 将视图更改为“测试”。
1. 在搜索字段中键入“10.192.64.56”
1. 在顶层筛选器的“范围”中，选择“源” 。

仅显示其中源 IP 是 10.192.64.56 的连接监视器（预览版）中失败的测试：
1. 将视图更改为“测试”。
1. 对于基于状态的筛选器，请选择“失败”。
1. 在搜索字段中键入“10.192.64.56”
1. 在顶层筛选器的“范围”中，选择“源” 。

仅显示其中目标为 outlook.office365.com 连接监视器（预览版）中失败的测试：
1. 将视图更改为“测试”。
1. 对于基于状态的筛选器，请选择“失败”。
1. 在搜索字段中，输入“outlook.office365.com”
1. 在顶层筛选器的“范围”中，选择“目标” 。

    :::image type="content" source="./media/connection-monitor-2-preview/tests-view.png" alt-text="显示筛选为仅显示 Outlook.Office365.com 目标测试失败的视图的屏幕截图" lightbox="./media/connection-monitor-2-preview/tests-view.png":::

若要了解连接监视器、测试组或测试的失败原因，请单击“原因”列。  该列会显示超过了哪个阈值（检查失败的百分比或 RTT）以及相关的诊断消息

:::image type="content" source="./media/connection-monitor-2-preview/cm-reason-of-failure.png" alt-text="显示连接监视器、测试或测试组失败原因的屏幕截图" lightbox="./media/connection-monitor-2-preview/cm-reason-of-failure.png":::

若要查看 RTT 的趋势和连接监视器检查失败的百分比，请执行以下操作：
1. 选择要调查的连接监视器。

    :::image type="content" source="./media/connection-monitor-2-preview/cm-drill-landing.png" alt-text="显示连接监视器的指标的屏幕截图（按测试组显示）" lightbox="./media/connection-monitor-2-preview/cm-drill-landing.png":::

1. 会看到以下部分  
    1. 概要 - 所选连接监视器的资源特定属性 
    1. 汇总 - 
        1. 对于连接监视器中的所有测试，RTT 的聚合趋势线和检查失败百分比。 可设置特定时间以查看详细信息。
        1. 基于 RTT 或检查失败百分比得出的测试组、源和目标中的前 5 项。 
    1. 测试组、源、目标和测试配置的选项卡 - 列出连接监视器中的测试组、源或目标。 失败的检查测试数，聚合 RTT 和检查失败百分比值。  还可回到过去的时间查看数据。 
    1. 问题 - 连接监视器中每个测试的跃点级别问题。 

        :::image type="content" source="./media/connection-monitor-2-preview/cm-drill-landing-2.png" alt-text="显示连接监视器的指标的屏幕截图（按测试组显示）第 2 部分" lightbox="./media/connection-monitor-2-preview/cm-drill-landing-2.png":::

1. 可以
    * 单击“查看所有测试”- 查看连接监视器中的所有测试
    * 单击“查看所有测试组、测试配置、源和目标”- 查看特定于每项的详细信息。 
    * 选择测试组、测试配置、源或目标 - 查看实体中的所有测试。

1. 在“查看所有测试”视图中，你可以：
    * 选择测试并单击“比较”。

        :::image type="content" source="./media/connection-monitor-2-preview/cm-compare-test.png" alt-text="显示 2 个测试的比较情况的屏幕截图" lightbox="./media/connection-monitor-2-preview/cm-compare-test.png":::

    * 使用群集将 VNET 和子网等复合资源扩展到其子资源
    * 单击拓扑查看任何测试的拓扑。

若要查看 RTT 的趋势和测试组检查失败的百分比，请执行以下操作：

1. 选择要调查的测试组。 
1. 你将看到与连接监视器类似的内容 - 有关于测试组、源、目标和测试配置的概要、汇总和表。 导航方式与连接监视器的一样

若要查看 RTT 的趋势和测试检查失败的百分比，请执行以下操作：
1. 选择要调查的测试。 你将看到网络拓扑，以及检查失败百分比和往返时间的端到端趋势图。 若要查看标识的问题，请在拓扑中选择路径中的任何跃点。 （这些跃点都是 Azure 资源。）此功能当前不适用于本地网络

  :::image type="content" source="./media/connection-monitor-2-preview/cm-test-topology.png" alt-text="显示测试的拓扑视图的屏幕截图" lightbox="./media/connection-monitor-2-preview/cm-test-topology.png":::

#### <a name="log-queries-in-log-analytics"></a>Log Analytics 中的日志查询

使用 Log Analytics 创建监视数据的自定义视图。 UI 显示的所有数据都来自 Log Analytics。 可以以交互方式分析存储库中的数据。 关联代理运行状况中的数据或基于 Log Analytics 的其他解决方案。 将数据导出到 Excel 或 Power BI，或创建可共享的链接。

#### <a name="metrics-in-azure-monitor"></a>Azure Monitor 中的指标

在连接监视器（预览版）体验推出之前创建的连接监视器中，所有四个指标都可用：探测失败、AverageRoundtripMs、ChecksFailedPercent（预览版）和 RoundTripTimeMs（预览版）。 在连接监视器（预览版）体验中创建的连接监视器中，数据仅可用于标记为（预览版）”的指标。

:::image type="content" source="./media/connection-monitor-2-preview/monitor-metrics.png" alt-text="显示连接监视器（预览版）中指标的屏幕截图" lightbox="./media/connection-monitor-2-preview/monitor-metrics.png":::

使用指标时，将资源类型设置为“Microsoft.Network/networkWatchers/connectionMonitors”

| 指标 | Display name | 计价单位 | 聚合类型 | 说明 | 维度 |
| --- | --- | --- | --- | --- | --- |
| ProbesFailedPercent | 失败的探测百分比 | 百分比 | 平均值 | 连接监视探测的失败百分比。 | 无维度 |
| AverageRoundtripMs | 平均值往返时间（毫秒） | 毫秒 | 平均值 | 源和目标之间发送的连接监视探测的平均网络 RTT。 |             无维度 |
| ChecksFailedPercent（预览版） | 失败检查（预览版） | 百分比 | 平均值 | 测试的检查失败百分比。 | ConnectionMonitorResourceId <br />SourceAddress <br />SourceName <br />SourceResourceId <br />SourceType <br />协议 <br />目的地地址 <br />DestinationName <br />DestinationResourceId <br />目标类型 <br />DestinationPort <br />TestGroupName <br />TestConfigurationName <br />区域 |
| RoundTripTimeMs（预览版） | 往返时间（毫秒）（预览版） | 毫秒 | 平均值 | 在源和目标之间发送的用于检查的 RTT。 此值不是平均值。 | ConnectionMonitorResourceId <br />SourceAddress <br />SourceName <br />SourceResourceId <br />SourceType <br />协议 <br />目的地地址 <br />DestinationName <br />DestinationResourceId <br />目标类型 <br />DestinationPort <br />TestGroupName <br />TestConfigurationName <br />区域 |

#### <a name="metric-based-alerts-for-connection-monitor"></a>连接监视器基于指标的警报

可使用以下方法在连接监视器上创建指标警报 

1. 通过连接监视器（预览版），在[使用 Azure 门户](connection-monitor-preview-create-using-portal.md#)创建连接监视器期间 
1. 通过连接监视器（预览版），在仪表板中使用“配置警报” 
1. 通过 Azure Monitor - 若要在 Azure Monitor 中创建警报： 
    1. 选择在连接监视器（预览版）中创建的连接监视器资源。
    1. 确保“指标”显示为连接监视器的信号类型。
    1. 在“信号名称”的“添加条件”中，选择“ChecksFailedPercent（预览版）”或“RoundTripTimeMs（预览版）”   。
    1. “信号类型”选择“指标” 。 例如，选择“ChecksFailedPercent（预览版）”。
    1. 将列出指标的所有维度。 选择维度名称和维度值。 例如，选择“源地址”，然后在连接监视器中输入任何源的 IP 地址。
    1. 在“警报逻辑”中填写以下详细信息：
        * 条件类型：静态。
        * 条件和阈值 。
        * 计算的聚合粒度和计算频率：连接监视器（预览版）每分钟更新一次数据。
    1. 在“操作”中选择操作组。
    1. 提供警报详细信息。
    1. 创建警报规则。

    :::image type="content" source="./media/connection-monitor-2-preview/mdm-alerts.jpg" alt-text="显示 Azure Monitor 中的“创建规则”区域的屏幕截图。突出显示了“源地址”和“源终结点名称”" lightbox="./media/connection-monitor-2-preview/mdm-alerts.jpg":::

## <a name="diagnose-issues-in-your-network"></a>诊断网络中的问题

连接监视器（预览版）可以帮助你诊断连接监视器中的问题和网络中的问题。 混合网络中的问题通过你之前安装的 Log Analytics 代理进行检测。 Azure 中的问题由网络观察程序扩展进行检测。 

可以在网络拓扑的 Azure 网络中查看问题。

对于源为本地 VM 的网络，可以检测到以下问题：

* 请求已超时。
* DNS 未解析终结点 - 暂时或永久。 URL 无效。
* 无法找到主机。
* 源无法连接到目标。 无法通过 ICMP 访问目标。
* 与证书相关的问题： 
    * 验证代理所需的客户端证书。 
    * 无法访问证书重定位列表。 
    * 终结点的主机名与证书的使用者或使用者备用名称不匹配。 
    * 源的本地计算机受信任的证书颁发机构存储中缺少根证书。 
    * SSL 证书已过期、无效、吊销或不兼容。

针对其源为Azure VM 的网络，可以检测到以下问题：

* 代理问题：
    * 代理已停止。
    * DNS 解析失败。
    * 无应用程序或在目标端口上侦听的侦听器。
    * 无法打开套接字。
* VM 状态问题： 
    * 正在启动
    * 正在停止
    * 已停止
    * 正在解除分配
    * 已解除分配
    * 正在重新启动
    * 未分配
* 缺少 ARP 表条目。
* 由于本地防火墙问题或 NSG 规则，流量遭到阻止。
* 虚拟网络网关问题： 
    * 缺少路由。
    * 两个网关之间的隧道已断开连接或处于缺失状态。
    * 第二个网关没有让隧道发现。
    * 无法找到对等信息。
* Microsoft Edge 中缺少路由。
* 由于系统路由或 UDR 的原因，流量已停止。
* 网关连接上未启用 BGP。
* 已关闭负载均衡器上的 DIP 探测。

## <a name="next-steps"></a>后续步骤

   * 了解[如何使用 Azure 门户创建连接监视器（预览版）](connection-monitor-preview-create-using-portal.md)  
   * 了解[如何使用 ARMClient 创建连接监视器（预览版）](connection-monitor-preview-create-using-arm-client.md)

<!-- Update_Description: new article about connection monitor overview -->
<!--NEW.date: 11/30/2020-->