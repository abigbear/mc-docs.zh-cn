---
title: 如何在 Kubernetes 上安装 IoT Edge | Microsoft Docs
description: 了解如何使用本地开发群集环境在 Kubernetes 上安装 IoT Edge
author: kgremban
manager: philmea
ms.author: v-tawe
origin.date: 04/26/2019
ms.date: 03/30/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: d13682f603861ff73af5f68bdf854259c8056b82
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80586694"
---
# <a name="how-to-install-iot-edge-on-kubernetes-preview"></a>如何在 Kubernetes 上安装 IoT Edge（预览）

IoT Edge 可与 Kubernetes 集成，将 Kubernetes 用作可复原的且高度可用的基础结构层。 下面是此支持在高级别 IoT Edge 解决方案中的适当位置：

![k8s 简介](./media/how-to-install-iot-edge-kubernetes/kubernetes-model.png)

>[!TIP]
>此集成的一个不错的心理模型是将 Kubernetes 视为除 Linux 和 Windows 外可供 IoT Edge 应用程序运行的另一个操作环境。

## <a name="architecture"></a>体系结构 
在 Kubernetes 上，IoT Edge 为边缘工作负载部署提供自定义资源定义  (CRD)。 IoT Edge 代理承担 CRD 控制器  的角色，协调云托管的所需状态与本地群集状态。

模块生存期由 Kubernetes 计划程序管理，该计划程序将维护模块的可用性及选择其位置。 IoT Edge 管理顶层运行的边缘应用程序平台，会持续将 IoT 中心内指定的所需状态与边缘群集上的状态相协调。 应用程序模型仍是用户熟悉的基于 IoT Edge 模块和路由的模型。 IoT Edge 代理控制器执行从 IoT Edge 的应用程序模型到 Kubernetes 本机构造（例如 Pod、部署、服务等）的自动转换。 

下面是概要性的体系结构示意图：

![kubernetes 体系结构](./media/how-to-install-iot-edge-kubernetes/publicpreview-refresh-kubernetes.png)

边缘部署的每个组件划归到特定于设备的 Kubernetes 命名空间，因而可在多个边缘设备及其部署之间共享相同的群集资源。

>[!NOTE]
>Kubernetes 上的 IoT Edge 以公共预览版提供。

## <a name="tutorials-and-references"></a>教程和参考 

有关详细信息（包括深度教程和参考），请参阅 [IoT Edge on Kubernetes 预览版文档迷你站点](https://microsoft.github.io/iotedge-k8s-doc/)。
