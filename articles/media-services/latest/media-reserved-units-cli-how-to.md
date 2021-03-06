---
title: 使用 CLI 缩放媒体预留单位 - Azure | Microsoft Docs
description: 本主题演示如何使用 CLI 通过 Azure 媒体服务来缩放媒体处理能力。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/09/2020
ms.date: 09/07/2020
ms.author: v-jay
ms.custom: seodec18
ms.openlocfilehash: 7829d902bb1499e785fb590f65084da502fe2288
ms.sourcegitcommit: 2eb5a2f53b4b73b88877e962689a47d903482c18
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/03/2020
ms.locfileid: "89413474"
---
# <a name="scaling-media-processing"></a>缩放媒体处理能力

使用 Azure 媒体服务，可以通过管理媒体预留单位 (MRU) 来缩放媒体处理能力。 MRU 确定处理媒体处理任务的速度。 可以在以下预留单位类型中进行选择：**S1**、**S2** 或 **S3**。 例如，与 **S1** 预留单位类型相比，使用 **S2** 预留单位类型时，同一编码作业运行速度更快。 

除了指定预留单位类型，还可以指定通过预留单位来设置帐户。 设置的预留单位数决定了给定帐户中可并发处理的媒体任务数。 例如，如果帐户具有 5 个预留单位，则只要有任务要处理，就可以同时运行 5 个媒体任务。 其余任务将排队等待，运行的任务完成后才选择它们以按顺序进行处理。 如果帐户未设置任何预留单位，则按顺序选择任务进行处理。 在这种情况下，完成一个任务和开始下一个任务之间的等待时间取决于系统中资源的可用性。

## <a name="choosing-between-different-reserved-unit-types"></a>在不同的预留单位类型之间进行选择

下表有助于在不同的编码速度之间进行选择时做出决定。 它还提供了几个基准案例，可[下载视频](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z)来执行自己的测试：

|RU 类型|方案|[7 分钟 1080p 视频](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z)的示例结果|
|---|---|---|
| **S1**|单比特率编码。 <br/>具有 SD 或更低分辨率的文件，不具有高时效性，成本低。|使用“H264 单比特率 SD 16x9”编码为单比特率 SD 分辨率 MP4 文件大约需要 7 分钟。|
| **S2**|单比特率和多比特率编码。<br/>SD 和 HD 编码的正常使用情况。|预设为“H264 单比特率 720p”的编码大约需要 6 分钟。<br/><br/>预设为“H264 多比特率 720p”的编码大约需要 12 分钟。|
| **S3**|单比特率和多比特率编码。<br/>全高清和 4K 分辨率视频。 对时间敏感，更快的编码周转。|预设为“H264 单比特率 1080p”的编码大约需要 3 分钟。<br/><br/>预设为“H264 多比特率 1080p”的编码大约需要 8 分钟。|

## <a name="considerations"></a>注意事项

* 如果使用共享的池（即没有任何预留单位），则编码任务将具有与 S1 RU 相同的性能。 但是，任务在排队状态下花费的时间可能没有上限，并且在任何给定的时间内，最多只会运行一项任务。

本文的其余部分介绍如何使用[媒体服务 v3 CLI](https://aka.ms/ams-v3-cli-ref) 来缩放 MRU。

## <a name="prerequisites"></a>先决条件 

[创建媒体服务帐户](./create-account-howto.md)。

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="scale-media-reserved-units-with-cli"></a>使用 CLI 缩放媒体预留单位

运行 `mru` 命令。

以下 [az ams account mru](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) 命令使用 **count** 和 **type** 参数在“amsaccount”帐户上设置媒体预留单位。

```azurecli
az ams account mru set -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>计费

根据在帐户中预配的媒体预留单位的分钟数计费。 这与帐户中是否有作业运行无关。 有关详细说明，请参阅[媒体服务定价](https://azure.cn/pricing/details/media-services/)页的“常见问题”部分。   

## <a name="see-also"></a>另请参阅

* [配额和限制](limits-quotas-constraints.md)
* [Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
