---
title: 对 Azure Cosmos DB 请求速率过大异常进行故障排除
description: 了解如何诊断和修复请求速率过大异常。
ms.service: cosmos-db
origin.date: 07/13/2020
author: rockboyfor
ms.date: 11/16/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: ce81e83d6af3564f19c879dc814d1a7aa5105d71
ms.sourcegitcommit: 5f07189f06a559d5617771e586d129c10276539e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94552291"
---
<!--Verified successfully-->
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-request-rate-too-large-exceptions"></a>诊断 Azure Cosmos DB 请求速率过大异常并对其进行故障排除
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

“请求速率过大”消息或错误代码 429 表明请求受到限制。

## <a name="troubleshooting-steps"></a>疑难解答步骤
以下部分包含请求过多的已知原因和解决方案。

### <a name="check-the-metrics"></a>检查指标
检查 [Azure Cosmos DB 监视](monitor-cosmos-db.md)，查看 429 异常的数量。

#### <a name="cause"></a>原因：
消耗的吞吐量（请求单位/秒）已超过[预配的吞吐量](set-throughput.md)。 SDK 会根据指定的重试策略自动重试请求。 如果经常遇到这种失败，请考虑增大集合的吞吐量。 检查门户的指标，确定是否收到了 429 错误。 查看分区键，以确保[均匀分配存储和请求量](partitioning-overview.md)。

#### <a name="solution"></a>解决方案：
1. 使用[门户或 SDK](set-throughput.md) 增加预配的吞吐量。
1. 将数据库或容器切换为[自动缩放](provision-throughput-autoscale.md)。

## <a name="next-steps"></a>后续步骤
* [诊断和排查](troubleshoot-dot-net-sdk.md)使用 Azure Cosmos DB .NET SDK 时遇到的问题。
* 了解 [.NET v3](performance-tips-dotnet-sdk-v3-sql.md) 和 [.NET v2](performance-tips.md) 的性能准则。

<!-- Update_Description: update meta properties, wording update, update link -->