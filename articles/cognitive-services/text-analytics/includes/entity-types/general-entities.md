---
title: 常规命名实体
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 10/26/2020
ms.author: v-johya
ms.openlocfilehash: 05b6ccc5feb379f645f5ff549e1a892eece53562
ms.sourcegitcommit: 93309cd649b17b3312b3b52cd9ad1de6f3542beb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93106577"
---
将请求发送到 `/entities/recognition/general` 终结点时，将返回以下实体类别。

| 类别   | Subcategory | 说明                          | 起始模型版本                                                    | 说明 |
|------------|-------------|--------------------------------------|-------------------------------------------------------------|--------------------------------------|
| 人员     | 空值         | 人员姓名。  | `2019-10-01`  | 也由 NER v2.1 返回 |
| PersonType | 空值         | 某人的工作类型或角色。 | `2020-02-01` | |
|位置    | 空值         | 自然地标和人造地标、结构、地理特征和地缘政治实体     |  `2019-10-01` | 也由 NER v2.1 返回 |
|位置     | 地缘政治实体 (GPE)        | 城市、国家/地区、省/自治区/直辖市。      | `2020-02-01` | |
|位置     | Structural                       | 人造结构。 | `2020-04-01` | |
|位置     | 地理       | 地理和自然特征，如河流、海洋和沙漠。 |  `2020-04-01` | |
|组织  | 空值 | 公司、政治团体、乐队、体育俱乐部、政府机构和公共组织。  | `2019-10-01` | 民族和宗教不包括在此实体类型中。 也由 NER v2.1 返回 |
|组织 | 医疗 | 医疗公司和团体。 | `2020-04-01` |  |
|组织 | 证券交易 | 证券交易所集团。 | `2020-04-01` | |
| 组织 | 体育游戏 | 与体育相关的组织。 | `2020-04-01` |  |
| 事件  | 空值 | 历史事件、社会事件和自然发生的事件。 | `2020-02-01` |  |
| 事件  | 文化 | 文化活动和假期。 | `2020-04-01` | |
| 事件  | Natural | 自然发生的事件。 | `2020-04-01` |  |
| 事件  | 体育游戏 | 体育活动。  | `2020-04-01` | |
| 产品 | 空值 | 各种类别的物理对象。 | `2020-02-01` | |
| 产品 | 计算产品 | 计算产品。 |  `2020-02-01 ` | |
| 技能 | 空值 | 能力、技能或专长。 | `2020-02-01` |  |
| 地址 | 不可用 | 完整的邮寄地址。  | `2020-04-01` |  |
| PhoneNumber | 空值 | 电话号码（仅限美国和欧洲电话号码）。 | `2019-10-01` | 也由 NER v2.1 返回 |
| 电子邮件 | 空值 | 电子邮件地址。 | `2019-10-01` | 也由 NER v2.1 返回 |
| 代码 | 空值 | 指向网站的 URL。 | `2019-10-01` | 也由 NER v2.1 返回  |
| IP | 空值 | 网络 IP 地址。 | `2019-10-01` | 也由 NER v2.1 返回 |
| DateTime | 空值 | 某天的日期和时间。 | `2019-10-01` | 也由 NER v2.1 返回 | 
| DateTime | Date | 日历日期。 | `2019-10-01` | 也由 NER v2.1 返回 |
| DateTime | 时间 | 一天中的时间 | `2019-10-01` | 也由 NER v2.1 返回 |
| DateTime | DateRange | 日期范围。 | `2019-10-01` | 也由 NER v2.1 返回 |
| DateTime | TimeRange | 时间范围。 | `2019-10-01` | 也由 NER v2.1 返回 |
| DateTime | 持续时间 | 持续时间。 | `2019-10-01` | 也由 NER v2.1 返回 |
| DateTime | 设置 | 集，重复的时间。 |  `2019-10-01` | 也由 NER v2.1 返回 |
| 数量 | 空值 | 数字和数量。 | `2019-10-01` | 也由 NER v2.1 返回  |
| 数量 | Number | 数字。 | `2019-10-01` | 也由 NER v2.1 返回 |
| 数量 | 百分比 | 百分比。| `2019-10-01` | 也由 NER v2.1 返回 |
| 数量 | Ordinal | 序号。 | `2019-10-01` | 也由 NER v2.1 返回 |
| 数量 | Age | 年龄。 | `2019-10-01` |  也由 NER v2.1 返回 |
| 数量 | 货币 | 货币。 | `2019-10-01` | 也由 NER v2.1 返回 |
| 数量 | 维度 | 维度和度量。 | `2019-10-01` | 也由 NER v2.1 返回 |
| 数量 | 温度 | 温度。 | `2019-10-01` | 也由 NER v2.1 返回 |

