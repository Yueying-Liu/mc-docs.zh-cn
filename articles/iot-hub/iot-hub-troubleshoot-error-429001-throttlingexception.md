---
title: 排查 Azure IoT 中心错误 429001 ThrottlingException
description: 了解如何修复错误 429001 ThrottlingException
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
origin.date: 01/30/2020
ms.date: 11/16/2020
ms.author: v-yiso
ms.openlocfilehash: f342539136bddbac5292b0d50ab42a8e63ef9ba9
ms.sourcegitcommit: 6b499ff4361491965d02bd8bf8dde9c87c54a9f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94328464"
---
# <a name="429001-throttlingexception"></a>429001 ThrottlingException

本文介绍 **429001 ThrottlingException** 错误的原因和解决方案。

## <a name="symptoms"></a>症状

对 IoT 中心的请求失败，出现错误 **429001 ThrottlingException**。

## <a name="cause"></a>原因

已超过所请求操作的 IoT 中心[限制](./iot-hub-devguide-quotas-throttling.md)。

## <a name="solution"></a>解决方案

根据上面指定的限制比较你的  “遥测消息发送尝试次数”指标，检查你是否达到了限制 还可以检查“限制错误数”  指标。 有关这些指标的信息，请参阅[设备遥测指标](monitor-iot-hub-reference.md#device-telemetry-metrics)。 有关如何使用指标来帮助你监视 IoT 中心的信息，请参阅[监视 IoT 中心](monitor-iot-hub.md)。

只有在违反限制的时间过长的情况下，IoT 中心才返回 429 ThrottlingException。 这样做以后，如果 IoT 中心获得突发流量，则不会丢弃消息。 在此期间，IoT 中心会按操作受限速率处理消息。如果积压了太多流量，则处理起来可能需要很长时间。 若要了解详细信息，请参阅 [IoT 中心流量调整](./iot-hub-devguide-quotas-throttling.md#traffic-shaping)。

## <a name="next-steps"></a>后续步骤

如果达到配额或节流限制，考虑[纵向扩展 IoT 中心](./iot-hub-scaling.md)。