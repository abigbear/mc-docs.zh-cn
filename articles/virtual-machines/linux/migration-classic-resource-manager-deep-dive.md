---
title: 有关平台支持的从经典部署模型到 Azure Resource Manager 的迁移的技术深入探讨
description: 对平台支持的从经典部署模型到 Azure 资源管理器的资源迁移进行技术深入探讨
services: virtual-machines-linux
documentationcenter: ''
author: Johnnytechn
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: 29267453-f894-4180-bb67-dce2a0e062bb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
origin.date: 03/30/2017
ms.date: 04/13/2020
ms.author: v-johya
ms.openlocfilehash: 5379842f23bae72e7aba93ca0e2ab676ec7c6e1d
ms.sourcegitcommit: ebedf9e489f5218d4dda7468b669a601b3c02ae5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2020
ms.locfileid: "82159196"
---
# <a name="technical-deep-dive-on-platform-supported-migration-from-classic-to-azure-resource-manager"></a>有关平台支持的从经典部署模型到 Azure Resource Manager 的迁移的技术深入探讨

> [!IMPORTANT]
> 目前，大约有 90% 的 IaaS VM 在使用 [Azure 资源管理器](https://www.azure.cn/home/features/resource-manager/)。 自 2020 年 2 月 28 日起，经典 VM 已弃用，并将于 2023 年 3 月 1 日完全停用。 [详细了解](https://docs.azure.cn/zh-cn/virtual-machines/classic-vm-deprecation)此弃用以及[它对你的影响](/virtual-machines/classic-vm-deprecation#how-does-this-affect-me)。

本文深入探讨如何从 Azure 经典部署模型迁移到 Azure Resource Manager 部署模型。 本文介绍资源和功能级别的资源，让用户了解 Azure 平台如何在两种部署模型之间迁移资源。 有关详细信息，请阅读服务公告文章：[平台支持的从经典部署模型到 Azure 资源管理器部署模型的 IaaS 资源迁移](migration-classic-resource-manager-overview.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)。

[!INCLUDE [virtual-machines-common-migration-deep-dive](../../../includes/virtual-machines-common-classic-resource-manager-migration-deep-dive.md)]

## <a name="next-steps"></a>后续步骤

* [平台支持的从经典部署模型到 Azure Resource Manager 部署模型的 IaaS 资源迁移概述](migration-classic-resource-manager-overview.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)
* [规划从经典部署模型到 Azure Resource Manager 的 IaaS 资源迁移](migration-classic-resource-manager-plan.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)
* [使用 PowerShell 将 IaaS 资源从经典部署模型迁移到 Azure 资源管理器](../windows/migration-classic-resource-manager-ps.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)
* [使用 CLI 将 IaaS 资源从经典部署模型迁移到 Azure 资源管理器](migration-classic-resource-manager-cli.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)
* [用于帮助将 IaaS 资源从经典部署模型迁移到 Azure 资源管理器部署模型的社区工具](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)
* [查看最常见的迁移错误](migration-classic-resource-manager-errors.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)
* [查看有关将 IaaS 资源从经典部署模型迁移到 Azure 资源管理器部署模型的最常见问题](migration-classic-resource-manager-faq.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)

<!-- Update_Description: update meta properties -->

