---
title: Azure 监视代理概述| Azure Docs
description: 本文提供可用于在 Azure 或混合环境中支持监视虚拟机的 Azure 代理的详细概述。
services: azure-monitor
origin.date: 02/14/2020
ms.subservice: ''
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
ms.date: 11/02/2020
ms.openlocfilehash: 0d2325750f5c6ce6f4ec82b11c76fe8001edbbef
ms.sourcegitcommit: 6b499ff4361491965d02bd8bf8dde9c87c54a9f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94328770"
---
# <a name="overview-of-azure-monitor-agents"></a>Azure Monitor 代理概述

虚拟机和其他计算资源需要代理收集所需的监视数据，以度量其来宾操作系统和工作负荷的性能和可用性。 本文介绍 Azure Monitor 使用的代理，并帮助你确定需要满足特定环境的哪些要求。

> [!NOTE]
> 由于 Azure Monitor 和 Log Analytics 最近已合并到一起，Azure Monitor 目前有多个代理。 虽然它们的特性可能存在重叠，但每个特性都有独特的功能。 可能需要在虚拟机上有一个或多个代理，具体取决于要求。 

你可能有一组特定的要求，这些要求在为特定虚拟机设置单个代理的情况下无法完全满足。 例如，你可能想要使用需要 Azure 诊断扩展的指标警报，但同时又想要利用用于 VM 的 Azure Monitor 功能，该功能需要 Log Analytics 代理和依赖项代理。 在这样的情况下，可以使用多个代理。对于需要每个代理中的功能的客户，这很常见。

## <a name="summary-of-agents"></a>代理摘要

下表对适用于 Windows 和 Linux 的 Azure Monitor 代理进行了快速比较。 以下部分提供了每个代理的更多详细信息。

> [!NOTE]
> Azure Monitor 代理目前以功能有限的预览版提供。 此表会进行更新 

### <a name="windows-agents"></a>Windows 代理

| | Azure Monitor 代理（预览版） | 诊断<br>扩展 (WAD) | Log Analytics<br>代理 | 依赖项<br>代理 |
|:---|:---|:---|:---|:---|
| **支持的环境** | Azure | Azure | Azure<br>其他云<br>本地 | Azure<br>其他云<br>本地 | 
| **代理要求**  | 无 | 无 | 无 | 需要 Log Analytics 代理 |
| **收集的数据** | 事件日志<br>性能 | 事件日志<br>ETW 事件<br>性能<br>基于文件的日志<br>IIS 日志<br>.NET 应用日志<br>故障转储<br>代理诊断日志 | 事件日志<br>性能<br>基于文件的日志<br>IIS 日志<br>见解和解决方案<br>其他服务 | 过程依赖项<br>网络连接指标 |
| **数据发送目标** | Azure Monitor 日志<br>Azure Monitor 指标 | Azure 存储<br>Azure Monitor 指标<br>事件中心 | Azure Monitor 日志 | Azure Monitor 日志<br>（通过 Log Analytics 代理） |
| **支持的**<br>**服务和**<br>**功能** | Log Analytics<br>指标资源管理器 | 指标资源管理器 | 用于 VM 的 Azure Monitor<br>Log Analytics<br>Azure 自动化<br>Azure 安全中心 | 用于 VM 的 Azure Monitor<br>服务映射 |

### <a name="linux-agents"></a>Linux 代理

| | Azure Monitor 代理（预览版） | 诊断<br>扩展 (LAD) | Telegraf<br>代理 | Log Analytics<br>代理 | 依赖项<br>代理 |
|:---|:---|:---|:---|:---|:---|
| **支持的环境** | Azure | Azure | Azure<br>其他云<br>本地 | Azure<br>其他云<br>本地 | Azure<br>其他云<br>本地 |
| **代理要求**  | 无 | 无 | 无 | 无 | 需要 Log Analytics 代理 |
| **收集的数据** | Syslog<br>性能 | Syslog<br>性能 | 性能 | Syslog<br>性能| 过程依赖项<br>网络连接指标 |
| **数据发送目标** | Azure Monitor 日志<br>Azure Monitor 指标 | Azure 存储<br>事件中心 | Azure Monitor 指标 | Azure Monitor 日志 | Azure Monitor 日志<br>（通过 Log Analytics 代理） |
| **支持的**<br>**功能**<br>**支持** | Log Analytics<br>指标资源管理器 | | 指标资源管理器 | 用于 VM 的 Azure Monitor<br>Log Analytics<br>Azure 自动化<br>Azure 安全中心 | 用于 VM 的 Azure Monitor<br>服务映射 |


<!--Not available in MC-->
## <a name="log-analytics-agent"></a>Log Analytics 代理

[Log Analytics 代理](log-analytics-agent.md)从 Azure 中的虚拟机、其他云提供商和本地的来宾操作系统与工作负荷收集监视数据。 它会将数据发送到 Log Analytics 工作区。 Log Analytics 代理是 System Center Operations Manager 使用的代理，你可以通过多宿主代理计算机同时与管理组和 Azure Monitor 通信。 Azure Monitor 和 Azure 中的其他服务中的某些见解也需要此代理。


> [!NOTE]
> 适用于 Windows 的 Log Analytics 代理通常称作 Microsoft Monitoring Agent (MMA)。 适用于 Linux 的 Log Analytics 代理通常称作 OMS 代理。



如果需要执行以下操作，请使用 Log Analytics 代理：

* 从 Azure 内部或外部的虚拟机或物理计算机收集日志和性能数据。 
* 将数据发送到 Log Analytics 工作区，以利用 [Azure Monitor 日志](data-platform-logs.md)支持的功能，例如[日志查询](../log-query/log-query-overview.md)。
* 使用 [Azure 安全中心](../../security-center/security-center-intro.md)管理虚拟机安全性。
* 使用 [Azure 自动化更新管理](../../automation/update-management/update-mgmt-overview.md)或 [Azure 自动化状态配置](../../automation/automation-dsc-overview.md)提供对 Azure VM 的全面管理
* 使用不同的[解决方案](../monitor-reference.md#insights-and-core-solutions)监视特定服务或者应用程序。

Log Analytics 代理的限制包括：

- 无法将数据发送到 Azure Monitor 指标、Azure 存储或 Azure 事件中心。
- 难以为单个代理配置唯一的监视定义。
- 难以大规模管理，因为每个虚拟机都有唯一的配置。

## <a name="azure-diagnostics-extension"></a>Azure 诊断扩展

[Azure 诊断扩展](diagnostics-extension-overview.md)从 Azure 虚拟机和其他计算资源的来宾操作系统和工作负载收集监视数据。 它主要将数据收集到 Azure 存储中，但也允许你通过定义数据接收器将数据同时发送到其他目标（例如 Azure Monitor 指标和 Azure 事件中心）。

如果需要执行以下操作，请使用 Azure 诊断扩展：

- 将数据发送到 Azure 存储进行存档，或使用 [Azure 存储资源管理器](../../vs-azure-tools-storage-manage-with-storage-explorer.md)之类的工具对其进行分析。
- 将数据发送到 [Azure Monitor 指标](data-platform-metrics.md)，以便使用[指标资源管理器](metrics-getting-started.md)对其进行分析，并利用准实时[指标警报](./alerts-metric-overview.md)和[自动缩放](autoscale-overview.md)之类的功能（仅限 Windows）。
- 使用 [Azure 事件中心](diagnostics-extension-stream-event-hubs.md)将数据发送到第三方工具。
- 收集[启动诊断](../../virtual-machines/troubleshooting/boot-diagnostics.md)数据，调查 VM 启动问题。

Azure 诊断扩展的限制包括：

- 只能与 Azure 资源配合使用。
- 将数据发送到 Azure Monitor 日志的功能有限。

## <a name="telegraf-agent"></a>Telegraf 代理

InfluxData Telegraf 代理用于将性能数据从 Linux 计算机收集到 Azure Monitor 指标。

如果需要执行以下操作，请使用 Telegraf 代理：

* 将数据发送到 [Azure Monitor 指标](data-platform-metrics.md)，以便使用[指标资源管理器](metrics-getting-started.md)对其进行分析，并利用准实时[指标警报](./alerts-metric-overview.md)和[自动缩放](autoscale-overview.md)之类的功能（仅限 Linux）。 



<!--Not available in MC: ../insights/vminsights-overview.md; ../insights/service-map.md-->
## <a name="virtual-machine-extensions"></a>虚拟机扩展

[Windows](../../virtual-machines/extensions/oms-windows.md) 和 [Linux](../../virtual-machines/extensions/oms-linux.md) 的 Log Analytics 扩展在 Azure 虚拟机上安装 Log Analytics 代理。 Windows 和 Linux 的 Azure Monitor 依赖项扩展在 Azure 虚拟机上安装 Dependency Agent。 它们是如上所述的代理，但你可以通过[虚拟机扩展](../../virtual-machines/extensions/overview.md)对它们进行管理。 应尽可能使用扩展来安装和管理代理。


## <a name="supported-operating-systems"></a>支持的操作系统
下表列出了 Azure Monitor 代理支持的操作系统。 请参阅每个代理的文档，以了解独特的注意事项和安装过程。 请参阅 Telegraf 文档以了解其支持的操作系统。 所有操作系统都假定为 x64。 任何操作系统均不支持 x86。

### <a name="windows"></a>Windows

| 操作系统 | Azure Monitor 代理 | Log Analytics 代理 | 依赖关系代理 | 诊断扩展 | 
|:---|:---:|:---:|:---:|:---:|
| Windows Server 2019                                      | X | X | X | X |
| Windows Server 2016                                      | X | X | X | X |
| Windows Server 2016 Core                                 |   |   |   | X |
| Windows Server 2012 R2                                   | X | X | X | X |
| Windows Server 2012                                      | X | X | X | X |
| Windows Server 2008 R2                                   |   | X | X | X |
| Windows 10 企业版<br>（包括多会话）和专业版<br>（仅限服务器方案）  | X | X | X | X |
| Windows 8 企业版和专业版<br>（仅限服务器方案）  |   | X | X |   |
| Windows 7 SP1<br>（仅限服务器方案）                 |   | X | X |   |


### <a name="linux"></a>Linux

| 操作系统 | Azure Monitor 代理 | Log Analytics 代理 | 依赖关系代理 | 诊断扩展 | 
|:---|:---:|:---:|:---:|:---:
| Amazon Linux 2017.09                                     |   | X |   |   |
| CentOS Linux 8                                           |   | X |   |   |
| CentOS Linux 7                                           | X | X |   | X |
| CentOS Linux 7.8                                         | X | X | X | X |
| CentOS Linux 7.6                                         | X | X | X | X |
| CentOS Linux 6                                           |   | X |   |   |
| CentOS Linux 6.5+                                        |   | X |   | X |
| Debian 10                                                | X |   |   |   |
| Debian 9                                                 | X | X | x | X |
| Debian 8                                                 |   | X | X | X |
| Debian 7                                                 |   |   |   | X |
| OpenSUSE 13.1+                                           |   |   |   | X |
| Oracle Linux 7                                           | X | X |   | X |
| Oracle Linux 6                                           |   | X |   |   |
| Oracle Linux 6.4+                                        |   | X |   | X |
| Red Hat Enterprise Linux Server 8                        |   | X |   |   |
| Red Hat Enterprise Linux Server 7                        | X | X | X | X |
| Red Hat Enterprise Linux Server 6                        |   | X | X |   |
| Red Hat Enterprise Linux Server 6.7+                     |   | X | X | X |
| SUSE Linux Enterprise Server 15                          | X | X |   |   |
| SUSE Linux Enterprise Server 12                          | X | X | X | X |
| Ubuntu 20.04 LTS                                         |   | X |   |   |
| Ubuntu 18.04 LTS                                         | X | X | X | X |
| Ubuntu 16.04 LTS                                         | X | X | X | X |
| Ubuntu 14.04 LTS                                         |   | X |   | X |


#### <a name="dependency-agent-linux-kernel-support"></a>Dependency Agent Linux 内核支持
由于 Dependency Agent 在内核级别工作，因此支持也依赖于内核版本。 下表列出了 Dependency Agent 的主要和次要 Linux OS 版本以及支持的内核版本。

| 分发 | OS 版本 | 内核版本 |
|:---|:---|:---|
|  Red Hat Linux 7   | 7.6     | 3.10.0-957  |
|                    | 7.5     | 3.10.0-862  |
|                    | 7.4     | 3.10.0-693  |
| Red Hat Linux 6    | 6.10    | 2.6.32-754 |
|                    | 6.9     | 2.6.32-696  |
| CentOSPlus         | 6.10    | 2.6.32-754.3.5<br>2.6.32-696.30.1 |
|                    | 6.9     | 2.6.32-696.30.1<br>2.6.32-696.18.7 |
| Ubuntu Server      | 18.04   | 5.3.0-1020<br>5.0（包括 Azure 优化内核）<br>4.18 *<br>4.15* |
|                    | 16.04.3 | 4.15.* |
|                    | 16.04   | 4.13.\*<br>4.11.\*<br>4.10.\*<br>4.8.\*<br>4.4.\* |
| SUSE Linux 12 Enterprise Server | 12 SP4 | 4.12.*（包括 Azure 优化内核） |
|                                 | 12 SP3 | 4.4.* |
|                                 | 12 SP2 | 4.4.* |
| Debian                          | 9      | 4.9  | 


## <a name="next-steps"></a>后续步骤

在以下站点获取每个代理的更多详细信息：

- [Log Analytics 代理概述](log-analytics-agent.md)
- [Azure 诊断扩展概述](diagnostics-extension-overview.md)
