---
title: Azure Batch 任务计划失败事件
description: Batch 任务计划失败事件参考。 当任务计划失败并且稍后将重试时，将发出此事件。
ms.topic: reference
origin.date: 09/20/2020
author: rockboyfor
ms.date: 11/02/2020
ms.testscope: yes|no
ms.testdate: 11/02/2020null
ms.author: v-yeche
ms.openlocfilehash: 36087112a71d195dc78a3517e45d25c27f788242
ms.sourcegitcommit: 93309cd649b17b3312b3b52cd9ad1de6f3542beb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93106496"
---
<!--Verified successfully for charactors-->
# <a name="task-schedule-fail-event"></a>任务计划失败事件

 当任务计划失败并且稍后将重试时，将发出此事件。 这是由于资源限制而导致的任务计划时间出现的暂时性失败，例如，节点上没有足够的槽用于运行指定了 `requiredSlots` 的任务。

 以下示例显示任务计划失败事件的正文。

```
{
    "jobId": "job-01",
    "id": "task-01",
    "taskType": "User",
    "systemTaskVersion": 665378862,
    "requiredSlots": 1,
    "nodeInfo": {
        "poolId": "pool-01",
        "nodeId": " "
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 0
    },
    "schedulingError": {
        "category": "UserError",
        "code": "JobPreparationTaskFailed",
        "message": "Task cannot run because the job preparation task failed on node"
    }
}
```

|元素名称|类型|说明|
|------------------|----------|-----------|
|`jobId`|字符串|包含任务的作业的 ID。|
|`id`|字符串|任务的 ID。|
|`taskType`|字符串|任务的类型。 它可以是“JobManager”（指示它是作业管理器任务），也可以是“User”（指示它并非作业管理器任务）。 对于作业准备任务、作业释放任务或开始任务，不会发出此事件。|
|`systemTaskVersion`|Int32|这是任务上的内部重试计数器。 批处理服务可能会在内部重试任务来解决暂时性问题。 这些问题可能包括内部计划错误或尝试恢复处于错误状态的计算节点。|
|`requiredSlots`|Int32|运行任务所需的槽。|
|[`nodeInfo`](#nodeInfo)|复杂类型|包含有关运行任务的计算节点的信息。|
|[`multiInstanceSettings`](#multiInstanceSettings)|复杂类型|指定任务是需要多个计算节点的多实例任务。  有关详细信息，请参阅 [`multiInstanceSettings`](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task)。|
|[`constraints`](#constraints)|复杂类型|应用到此任务的执行约束。|
|[`schedulingError`](#schedulingError)|复杂类型|包含有关任务计划错误的信息。|

### <a name="nodeinfo"></a><a name="nodeInfo"></a>nodeInfo

|元素名称|类型|说明|
|------------------|----------|-----------|
|`poolId`|字符串|运行任务的池的 ID。|
|`nodeId`|字符串|运行任务的节点的 ID。|

### <a name="multiinstancesettings"></a><a name="multiInstanceSettings"></a>multiInstanceSettings

|元素名称|类型|说明|
|------------------|----------|-----------|
|`numberOfInstances`|Int32|任务所需的计算节点数。|

### <a name="constraints"></a><a name="constraints"></a>constraints

|元素名称|类型|说明|
|------------------|----------|-----------|
|`maxTaskRetryCount`|Int32|可以重试任务的最大次数。 批处理服务在其退出代码非零时重试任务。<br /><br /> 请注意，此值专门用于控制重试的次数。 批处理服务将尝试任务一次，然后重试，直至达到此上限为止。 例如，如果最大重试计数为 3，则批处理任务最多尝试任务 4 次（一次是初始尝试，其余 3 次是重试）。<br /><br /> 如果最大重试计数为 0，则批处理服务不会重试任务。<br /><br /> 如果最大重试计数为 -1，则批处理服务会无限制地重试任务。<br /><br /> 默认值为 0（不重试）。|

<a name="schedulingError"></a>
### <a name="schedulingerror"></a>schedulingError

|元素名称|类型|说明|
|------------------|----------|-----------|
|`category`|字符串|错误的类别。|
|`code`|字符串|任务计划错误的标识符。 代码是固定的，旨在以编程方式使用。|
|`message`|字符串|描述任务计划错误的消息，旨在适合在用户界面中显示。|

<!-- Update_Description: update meta properties, wording update, update link -->