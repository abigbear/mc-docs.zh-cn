---
title: Azure VM 大小 - 计算优化 | Azure
description: 列出了 Azure 中虚拟机可用的不同计算优化大小。 列出了有关此系列中各大小的 vCPU 数、数据磁盘数和 NIC 数以及存储吞吐量和网络带宽的信息。
author: rockboyfor
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.workload: infrastructure-services
origin.date: 02/03/2020
ms.date: 08/31/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 5a0f7450b346e4cee95c316a3a1683f768207190
ms.sourcegitcommit: 63a4bc7c501fb6dd54a31d39c87c0e8692ac2eb0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "89052440"
---
<!--Verified successfully-->
<!--Partical Content from verified-->
# <a name="compute-optimized-virtual-machine-sizes"></a>计算优化虚拟机大小

计算优化 VM 大小具有较高的 CPU 与内存之比。 这些大小适用于中等流量的 Web 服务器、网络设备、批处理和应用程序服务器。 本文提供了有关 vCPU、数据磁盘和 NIC 的数量的信息。 它还介绍了此分组中每个大小的存储吞吐量和网络带宽。

[Fsv2 系列](fsv2-series.md)在第 2 代英特尔® 至强® 铂金 8272CL (Cascade Lake) 处理器和英特尔® 至强® 铂金 8168 (Skylake) 处理器上运行。 它具有稳定的 3.4 GHz 的全核 Turbo 时钟速度和最大为 3.7 GHz 的单核 Turbo 频率。 Intel 可扩展处理器上提供了全新的 Intel® AVX-512 指令。 对于单精度和双精度浮点运算，这些指令可为向量处理工作负荷提供高达 2 倍的性能提升。 换而言之，对于任何计算工作负荷，它们的处理速度相当快。

凭借较低的每小时定价，Fsv2 系列在基于每个 vCPU 的 Azure 计算单位 (ACU) 的 Azure 产品组合中具有最高性价比。

## <a name="other-sizes"></a>其他大小

- [常规用途](sizes-general.md)
- [内存优化](sizes-memory.md)

    <!--Not Available on - [Storage optimized](sizes-storage.md)-->
    
- [GPU 优化](sizes-gpu.md)
    
    <!--Not Available on - [High performance compute](sizes-hpc.md)-->
    
- [前几代](sizes-previous-gen.md)

## <a name="next-steps"></a>后续步骤

了解有关 [Azure 计算单元 (ACU)](acu.md) 如何帮助跨 Azure SKU 比较计算性能的详细信息。

有关 Azure 如何命名其 VM 的详细信息，请参阅 [Azure 虚拟机大小命名约定](./vm-naming-conventions.md)。

<!-- Update_Description: update meta properties, wording update, update link -->