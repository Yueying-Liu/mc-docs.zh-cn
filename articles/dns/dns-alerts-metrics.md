---
title: 指标和警报 - Azure DNS
description: 借助此学习路径，开始了解 Azure DNS 指标和警报。
services: dns
documentationcenter: na
author: WenJason
manager: digimobile
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 04/17/2018
ms.date: 01/13/2020
ms.author: v-jay
ms.openlocfilehash: 4fdada8f245eb450a95c947b5ae5782ba143c7e8
ms.sourcegitcommit: 39288459139a40195d1b4161dfb0bb96f5b71e8e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94590770"
---
# <a name="azure-dns-metrics-and-alerts"></a>Azure DNS 指标和警报
Azure DNS 是 DNS 域的托管服务，它使用 Azure 基础结构提供名称解析。 本文介绍了 Azure DNS 服务的指标和警报。

## <a name="azure-dns-metrics"></a>Azure DNS 指标

Azure DNS 为客户提供指标，帮助他们监视服务中托管的 DNS 区域的具体各方面。 此外，借助 Azure DNS 指标，可以基于利害条件配置和接收警报。 指标通过 [Azure Monitor 服务](../azure-monitor/index.yml)提供。 Azure DNS 通过 Azure Monitor 为 DNS 区域提供以下指标：

-   QueryVolume
-   RecordSetCount
-   RecordSetCapacityUtilization

还可以在 Azure Monitor 文档页看到[这些指标的定义](../azure-monitor/platform/metrics-supported.md#microsoftnetworkdnszones)。
>[!NOTE]
> 这个时候，这些指标仅可用于 Azure DNS 中托管的公共 DNS 区域。 如果 Azure DNS 托管专用区域，那么这些指标不为这些区域提供数据。 

可以看到指标的最细粒度元素是 DNS 区域。 当前无法查看区域中单个资源记录的指标。

### <a name="query-volume"></a>查询量

Azure DNS 中的“查询量”指标显示 Azure DNS 接收到的 DNS 区域的 DNS 查询量（查询流量）  。 度量单位为计数，聚合是一段时间内收到的查询总数。 

若要查看此指标，请从 Azure 门户的“监视”选项卡选择“指标(预览)”资源管理器体验。 从“资源”下拉列表选择 DNS 区域，选择“查询量”指标，然后选择“总和”作为“聚合”。 下面的屏幕快照显示示例。  有关指标资源管理器体验和图表的详细信息，请参阅 [Azure Monitor 指标资源管理器](../azure-monitor/platform/metrics-charts.md)。

![查询量](./media/dns-alerts-metrics/dns-metrics-query-volume.png)

图：Azure DNS 查询量指标 

### <a name="record-set-count"></a>记录集计数
“记录集计数”指标显示 Azure DNS 中 DNS 区域的记录集数  。 对区域中定义的所有记录集进行计数。 度量单位是计数，聚合是所有记录集最大值。 若要查看此指标，请从 Azure 门户的“监视”选项卡选择“指标(预览)”资源管理器体验   。 从“资源”下拉列表选择 DNS 区域，选择“记录集计数”指标，然后选择“最大值”作为“聚合”     。 有关指标资源管理器体验和图表的详细信息，请参阅 [Azure Monitor 指标资源管理器](../azure-monitor/platform/metrics-charts.md)。 

![记录集计数](./media/dns-alerts-metrics/dns-metrics-record-set-count.png)

图：Azure DNS 记录集计数指标 


### <a name="record-set-capacity-utilization"></a>记录集容量使用率
Azure DNS 的“记录集容量使用率”指标显示 DNS 区域的记录集容量使用百分比  。 Azure DNS 中的每个 DNS 区域受到记录集限制的约束，该限制定义区域允许的记录集最大数（请参阅 [DNS 限制](dns-zones-records.md#limits)）。 因此，此指标显示命中记录集限制的接近程度。 例如，如果为 DNS 区域配置了 500 个记录集，并且区域的默认记录集限制为 5000，那么 RecordSetCapacityUtilization 指标的值将显示为 10%（通过 500 除以 5000 计算得出）。 度量单位是百分比，聚合类型是最大值    。 若要查看此指标，请从 Azure 门户的“监视”选项卡选择“指标(预览)”资源管理器体验。 从“资源”下拉列表选择 DNS 区域，选择“记录集容量使用率”指标，然后选择“最大值”作为“聚合”。 下面的屏幕快照显示示例。 有关指标资源管理器体验和图表的详细信息，请参阅 [Azure Monitor 指标资源管理器](../azure-monitor/platform/metrics-charts.md)。 

![显示如何查看指标的示例的屏幕截图。](./media/dns-alerts-metrics/dns-metrics-record-set-capacity-uitlization.png)

图：Azure DNS 记录集容量使用率指标 

## <a name="alerts-in-azure-dns"></a>Azure DNS 中的警报
Azure Monitor 提供针对可用指标值进行报警的功能。 新警报配置体验中提供 DNS 指标。 如 [Azure Monitor 警报文档](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md)中所述，可以选择 DNS 区域作为资源，选择“指标”信号类型，并配置警报逻辑和其他参数（例如“周期”和“频率”）   。 满足警报条件时，可以进一步定义[操作组](../azure-monitor/platform/action-groups.md)，借此警报将通过所选操作传送。 有关如何配置 Azure Monitor 指标报警的详细信息，请参阅[使用 Azure Monitor 创建、查看和管理警报](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md)。 

## <a name="next-steps"></a>后续步骤
- 详细了解 [Azure DNS](dns-overview.md)。
