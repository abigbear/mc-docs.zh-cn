---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 06/23/2020
ms.date: 07/20/2020
ms.author: v-jay
ms.openlocfilehash: f87f71ca2b6b29d258f77058a24d33bfc0f79211
ms.sourcegitcommit: 4e2d781466e54e228fd1dbb3c0b80a1564c2bf7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2020
ms.locfileid: "86414978"
---
| 资源 | 目标 | 目标（预览版） |
|-|-|-|
| 单个 Blob 容器的最大大小 | 与最大存储帐户容量相同 |  |
| 块 Blob 或附加 Blob 中的块数上限 | 50,000 块 |  |
| 块 Blob 中块的最大大小 | 100 MiB | 4000 MiB（预览版） |
| 块 Blob 的最大大小 | 50,000 X 100 MiB（大约 4.75 TiB） | 50,000 X 4000 MiB（大约 190.7 TiB）（预览版） |
| 附加 Blob 中块的最大大小 | 4 MiB |  |
| 附加 Blob 的最大大小 | 50,000 x 4 MiB（大约 195 GiB） |  |
| 页 Blob 的最大大小 | 8 TiB |  |
| 每个 Blob 容器存储的访问策略的最大数目 | 5 |  |
| 单个 blob 的目标请求速率 | 每秒最多 500 个请求 |  |
| 单个页 blob 的目标吞吐量 | 最高每秒 60 MiB |  |
| 单个块 blob 的目标吞吐量 | 上限为存储帐户的传入/传出限制<sup>1</sup> |  |

<sup>1</sup> 单个 blob 的吞吐量取决于多个因素，包括但不限于：并发性、请求大小、性能层、源上传速度和目标下载速度。 具体地说，对于标准存储帐户，请使用大于 4 MiB 的 Blob 或块大小调用 [Put Blob](https://docs.microsoft.com/rest/api/storageservices/put-blob) 或 [Put Block](https://docs.microsoft.com/rest/api/storageservices/put-block) 操作。 对于高级块 blob 或 Data Lake Storage Gen2 存储帐户，请使用大于 256 KiB 的块或 blob 大小。

下表描述了服务版本允许的最大块大小和 blob 大小。

| 服务版本 | 最大块大小（通过放置块） | 最大 blob 大小（通过放置块列表） | 通过单个写入操作的最大 blob 大小（通过放置 Blob） |
|-|-|-|-|
| 版本 2019-12-12 和更高版本 | 4000 MiB（预览版） | 大约 190.7 TiB（4000 MiB X 50,000 块）（预览版） | 5000 MiB（预览版） |
| 版本 2016-05-31 到版本 2019-07-07 | 100 MiB | 大约 4.75 TiB（100 MiB X 50,000 块） | 256 MiB |
| 2016-05-31 之前的版本 | 4 MiB | 大约 195 GiB（4 MiB X 50,000 块） | 64 MiB |
