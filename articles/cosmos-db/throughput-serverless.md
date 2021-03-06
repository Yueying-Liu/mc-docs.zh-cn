---
title: 如何在 Azure Cosmos DB 上的预配吞吐量与无服务器之间进行选择
description: 了解如何针对你的工作负荷在预配吞吐量与无服务器之间进行选择。
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 08/19/2020
author: rockboyfor
ms.date: 11/16/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: df21bcd33d63d0b34ea245110a92c78adf0017b4
ms.sourcegitcommit: 5f07189f06a559d5617771e586d129c10276539e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94552669"
---
<!--Verified Successfully-->
# <a name="how-to-choose-between-provisioned-throughput-and-serverless"></a>如何在预配吞吐量与无服务器之间进行选择
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB 提供了两种不同的容量模式：[预配吞吐量](set-throughput.md)和[无服务器](serverless.md)。 在这两种模式下，你可以执行完全相同的数据库操作，但这些操作的收费方式截然不同。

<!--MOONCAKE CUSTOMIZATION REMOVE: The following video explains the core differences between these modes and how they fit different types of workloads:-->
<!--Not Available on [!VIDEO https://www.youtube.com/embed/CgYQo6uHyt0]-->

> [!NOTE]
> 目前只有 Azure Cosmos DB Core (SQL) API 支持无服务器。

## <a name="detailed-comparison"></a>详细比较

| 条件 | 预配的吞吐量 | 无服务器 |
| --- | --- | --- |
| 状态 | 正式发布 | 预览 |
| 最适用于 | 需要可预测性能的关键工作负荷 | 流量低的小到中型非关键工作负荷 |
| 工作原理 | 对于每个容器，你预配一定数量的吞吐量（以每秒[请求单位数](request-units.md)表示）。 每秒此数量的请求单位可用于你的数据库操作。 预配的吞吐量可以手动更新，也可以通过[自动缩放](provision-throughput-autoscale.md)自动调整。 | 你对容器运行数据库操作，无需预配任何容量。 |
| 每个帐户的限制 | Azure 区域的最大数量：不受限制 | Azure 区域的最大数量：1 |
| 每个容器的限制 | 最大吞吐量：不受限制<br />最大存储：不受限制 | 最大吞吐量：5,000 RU/秒<br />最大存储：50 GB |
| 性能 | SLA 涵盖的可用性为 99.99% 到 99.999%<br />SLA 涵盖的针对点读取和写入的延迟 < 10 毫秒<br />SLA 涵盖的保证吞吐量为 99.99% | SLA 涵盖的可用性为 99.9% 到 99.99%<br />SLA 涵盖的针对点读取的延迟 < 10 毫秒，针对写入的延迟 < 30 毫秒<br />SLA 涵盖的突发性为 95% |
| 计费模式 | 对于预配的 RU/s，无论消耗了多少 RU，都按每小时计费。 | 根据数据库操作所消耗的 RU 数量，按小时收费。 |

> [!IMPORTANT]
> 当无服务器正式发布时，一些无服务器限制可能会被解除或删除，**你的反馈** 将有助于我们做出决定！ 请与我们联系，并告诉我们有关无服务器体验的详细信息：[azurecosmosdbserverless@service.microsoft.com](mailto:azurecosmosdbserverless@service.microsoft.com)。

## <a name="burstability-and-expected-consumption"></a>突发性和预期消耗

在某些情况下，可能无法确定应当为给定的工作负荷选择预配吞吐量还是选择无服务器。 为了做出此决策，你可以估计：

- 你的工作负荷的 **突发性** 需求，即一秒内可能需要消耗的最大 RU 数量

<!--Not Available on - Your overall **expected consumption**, that is what's the total number of RUs you may consume over a month (you can estimate this with the help of the table shown here)-->
<!--Not Available on [here](plan-manage-costs.md#estimating-serverless-costs)-->

如果工作负荷的突发性需求超出每秒 5,000 RU，则应选择预配吞吐量，因为无服务器容器不能超过此限制。 如果不是，则可以根据预期的消耗来比较这两种模式的成本。

**示例 1**：工作负荷预计的最大突发需求为 10,000 RU/秒，并且一个月内总共使用 20,000,000 个 RU。

- 只有预配吞吐量模式可以提供 10,000 RU/秒的吞吐量

<!--Not Available on Serverless price-->


> [!NOTE]
> 上面示例中所示的成本仅用于演示目的。 有关最新定价信息，请参阅[定价页](https://www.azure.cn/pricing/details/cosmos-db/)。

## <a name="next-steps"></a>后续步骤

- 详细了解如何[在 Azure Cosmos DB 上预配吞吐量](set-throughput.md)
- 详细了解 [Azure Cosmos DB 无服务器](serverless.md)
- 熟悉[请求单位](request-units.md)的概念

<!-- Update_Description: update meta properties, wording update, update link -->