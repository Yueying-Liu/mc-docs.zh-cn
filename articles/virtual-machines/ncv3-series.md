---
title: NCv3 系列 - Azure 虚拟机
description: NCv3 系列 VM 的规范。
ms.subservice: sizes
ms.service: virtual-machines
ms.topic: conceptual
origin.date: 02/03/2020
author: rockboyfor
ms.date: 11/02/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 5915080d6e24932cdf6a1e93f3879efb1c821d82
ms.sourcegitcommit: 93309cd649b17b3312b3b52cd9ad1de6f3542beb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93104299"
---
# <a name="ncv3-series"></a>NCv3 系列

NCv3 系列 VM 采用 NVIDIA Tesla V100 GPU。 客户可将这些更新的 GPU 用于传统的 HPC 工作负荷，例如油藏模拟、DNA 测序、蛋白质分析、Monte Carlo 模拟和其他工作负荷。 NC24rs v3 配置提供了针对紧密耦合的并行计算工作负荷优化的低延迟、高吞吐量网络接口。 除了 GPU 之外，NCv3 系列 VM 还采用 Intel Xeon E5-2690 v4 (Broadwell) CPU。

<!--Not Available on These GPUs can provide 1.5x the computational performance of the NCv2-series-->
<!--Not Available on HPC-->

[高级存储](premium-storage-performance.md)：支持<br />
[高级存储缓存](premium-storage-performance.md)：支持<br />
[实时迁移](maintenance-and-updates.md)：不支持<br />
[内存保留更新](maintenance-and-updates.md)：不支持<br />
[VM 代系支持](generation-2.md)：第 1 代和第 2 代<br />

> [!IMPORTANT]
> 对于此 VM 系列，订阅中的 vCPU（核心）配额最初在每个区域中设置为 0。 在[可用区域](https://azure.microsoft.com/regions/services/)中为此系列[请求 vCPU 配额增加](https://support.azure.cn/support/support-azure/)。 这些 SKU 不可用于试用版或 Visual Studio 订阅服务器 Azure 订阅。 你的订阅级别可能不支持选择或部署这些 SKU。 
>

| 大小 | vCPU | 内存:GiB | 临时存储 (SSD) GiB | GPU | GPU 内存：GiB | 最大数据磁盘数 | 最大非缓存磁盘吞吐量：IOPS/MBps | 最大 NIC 数 |
|---|---|---|---|---|---|---|---|---|
| Standard_NC6s_v3    | 6  | 112 | 736  | 1 | 16 | 12 | 20000/200 | 4 |
| Standard_NC12s_v3   | 12 | 224 | 1474 | 2 | 32 | 24 | 40000/400 | 8 |
| Standard_NC24s_v3   | 24 | 448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |
| Standard_NC24rs_v3* | 24 | 448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |

1 GPU = 一个 V100 卡。

*支持 RDMA

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>支持的操作系统和驱动程序

若要利用 Azure N 系列 VM 的 GPU 功能，必须安装 NVIDIA GPU 驱动程序。

<!--Not Available on The NVIDIA GPU Driver Extension installs appropriate NVIDIA CUDA or GRID drivers on an N-series VM. Install or manage the extension using the Azure portal or tools such as Azure PowerShell or Azure Resource Manager templates. See the NVIDIA GPU Driver Extension documentation for supported operating systems and deployment steps.-->
<!--Not Available on [NVIDIA GPU Driver Extension](./extensions/hpccompute-gpu-windows.md)-->
<!--Not Capatile on [Azure virtual machine extensions and features](./extensions/overview.md)-->

选择手动安装 NVIDIA GPU 驱动程序时，请参阅[适用于 Windows 的 N 系列 GPU 驱动程序安装](./windows/n-series-driver-setup.md)或[适用于 Linux 的 N 系列 GPU 驱动程序安装](./linux/n-series-driver-setup.md)，了解受支持的操作系统、驱动程序以及安装和验证步骤。

## <a name="other-sizes"></a>其他大小

- [常规用途](sizes-general.md)
- [内存优化](sizes-memory.md)
    
    <!--Not Available on - [Storage optimized](sizes-storage.md)-->
    
- [GPU 优化](sizes-gpu.md)
    
    <!--Not Available on - [High performance compute](sizes-hpc.md)-->
    
- [前几代](sizes-previous-gen.md)

## <a name="next-steps"></a>后续步骤

了解有关 [Azure 计算单元 (ACU)](acu.md) 如何帮助跨 Azure SKU 比较计算性能的详细信息。

<!-- Update_Description: update meta properties, wording update, update link -->