---
title: 如何使用 Commvault 在 Azure Stack Hub 上备份 VM
description: 了解如何使用 Commvault 在 Azure Stack Hub 上备份 VM。
author: WenJason
ms.topic: how-to
origin.date: 04/20/2020
ms.date: 08/31/2020
ms.author: v-jay
ms.reviewer: sijuman
ms.lastreviewed: 10/30/2019
ms.openlocfilehash: 6a8ba63c7eba59316e7fd4210547eb6026b6f3c3
ms.sourcegitcommit: 4e2d781466e54e228fd1dbb3c0b80a1564c2bf7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/26/2020
ms.locfileid: "88867953"
---
# <a name="back-up-your-vm-on-azure-stack-hub-with-commvault"></a>使用 Commvault 在 Azure Stack Hub 上备份 VM

## <a name="overview-of-backing-up-a-vm-with-commvault"></a>有关使用 Commvault 备份 VM 的概述

本文逐步讲解如何配置 Commvault Live Sync，以更新位于独立 Azure Stack Hub 缩放单元上的恢复 VM。 本文将会详述如何配置常见的合作伙伴解决方案，以保护和恢复 Azure Stack Hub 上部署的虚拟机的数据与系统状态。

下图显示了使用 Commvault 备份 VM 时的总体解决方案。

![](./media/azure-stack-network-howto-backup-commvault/bcdr-commvault-overall-arc.png)

本文内容：

1. 在源 Azure Stack Hub 实例上创建运行 Commvault 软件的 VM。

2. 在辅助位置中创建一个存储帐户。 本文假设在独立 Azure Stack Hub 实例（目标）上的存储帐户中创建 Blob 容器，并且源 Azure Stack Hub 可以访问目标 Azure Stack Hub。

3. 在源 Azure Stack Hub 实例上配置 Commvault，并将源 Azure Stack Hub 中的 VM 添加到 VM 组。

4. 配置 Commvault 的 Live Sync。

也可以下载并提供兼容的合作伙伴 VM 映像，以通过 Azure 云或另一个 Azure Stack Hub 保护 Azure Stack Hub VM。 本文将会演示如何使用 Commvault Live Sync 来保护 VM。

此方法的拓扑如下图所示：

![](./media/azure-stack-network-howto-backup-commvault/backup-vm-commvault-diagram.svg)

## <a name="create-the-commvault-vm-from-the-commvault-marketplace-item"></a>从 Commvault 市场项创建 Commvault VM

1. 打开 Azure Stack Hub 用户门户。

2. 选择“创建资源” > “计算” > “Commvault”。  

    > [!Note]  
    > 如果 Commvault 不可用，请与云操作员联系。

    ![创建 VM](./media/azure-stack-network-howto-backup-commvault/commvault-create-vm-01.png)

3. 在“创建虚拟机，1 基本信息”中配置基本设置：

    a. 输入“名称”。

    b. 选择“标准 HDD”。
    
    c. 输入**用户名**。
    
    d. 输入**密码**。
    
    e. 确认密码。
    
    f. 选择用于备份的**订阅**。
    
    g. 选择一个**资源组**。
    
    h.如果该值不存在，请单击“添加行”。 选择 Azure Stack Hub 的**位置**。 如果使用的是 ASDK，请选择“本地”。
    
    i. 选择“确定” 。

    ![](./media/azure-stack-network-howto-backup-commvault/commvault-create-vm-02.png)

4. 选择 Commvault VM 的大小。 用于备份的 VM 大小应至少有 10 GB 的 RAM，以及 100 GB 的存储。

    ![](./media/azure-stack-network-howto-backup-commvault/commvault-create-vm-03.png)。

5. 选择 Commvault VM 的设置。

    a. 将可用性设置为“无”。
    
    b. 对于“使用托管磁盘”，请选择“是”。
    
    c. 在“虚拟网络”中选择默认的 VNet。
    
    d. 选择默认的“子网”。
    
    e. 选择默认的“公共 IP 地址”。
    
    f. 将 VM 保留在“基本”网络安全组中。
    
    g. 打开 HTTP (80)、HTTPS (443)、SSH (22) 和 RDP (3389) 端口。
    
    h.如果该值不存在，请单击“添加行”。 选择“无扩展”。
    
    i. 对于“启动诊断”，请选择“已启用”。 
    
    j. 将“来宾 OS 诊断”保持设置为“已禁用”。 
    
    k. 保留默认的“诊断存储帐户”。
    
    l. 选择“确定” 。

6. 在 Commvault VM 通过验证后，查看其摘要。 选择“确定” 。

## <a name="get-your-service-principal"></a>获取服务主体

需要知道标识管理器是 Azure AD 还是 ADFS。 下表包含在 Azure Stack Hub 中设置 Commvault 时所需的信息。

| 元素 | 说明 | Source |
|--------------------------|--------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Azure 资源管理器 URL | Azure Stack Hub 资源管理器终结点。 | https://docs.azure.cn/azure-stack/user/azure-stack-version-profiles-ruby?view=azs-1908#the-azure-stack-hub-resource-manager-endpoint |
| 应用程序名称 |  |  |
| 应用程序 ID | 在本文上一部分创建服务主体时保存的服务主体应用 ID。 | https://docs.azure.cn/azure-stack/operator/azure-stack-create-service-principals?view=azs-1908 |
| 订阅 ID | 使用订阅 ID 访问 Azure Stack Hub 中的套餐。 | https://docs.azure.cn/azure-stack/operator/service-plan-offer-subscription-overview?view=azs-1908#subscriptions |
| 租户 ID（目录 ID） | Azure Stack Hub 租户 ID。 | https://docs.azure.cn/azure-stack/operator/azure-stack-identity-overview?view=azs-1908 |
| 应用程序密码 | 创建服务主体时保存的服务主体应用机密。 | https://docs.azure.cn/azure-stack/operator/azure-stack-create-service-principals?view=azs-1908 |

## <a name="configure-backup-using-the-commvault-console"></a>使用 Commvault 控制台配置备份

1. 打开 RDP 客户端，并连接到 Azure Stack Hub 中的 Commavult VM。 输入凭据。

2. 在 Commvault VM 上安装 Azure Stack Hub PowerShell 和 Azure Stack Hub 工具。

    a. 有关安装 Azure Stack Hub PowerShell 的说明，请参阅[安装适用于 Azure Stack Hub 的 PowerShell](../operator/azure-stack-powershell-install.md)。  
    b. 有关安装 Azure Stack Hub 工具的说明，请参阅[从 GitHub 下载 Azure Stack Hub 工具](../operator/azure-stack-powershell-download.md)。

3. 在 Commvault 安装到 Commvault VM 后，打开 Commcell 控制台。 在“开始”中，选择“Commvault” > “Commvault Commcell 控制台”。 

    ![](./media/azure-stack-network-howto-backup-commvault/commcell-console.png)

4. 在 Commvault Commcell 控制台中，将备份存储库配置为使用 Azure Stack Hub 外部的存储。 在 CommCell 浏览器中，选择“存储资源”>“存储池”。 单击右键并选择“添加存储池”。 选择“云”。

5. 添加存储池的名称。 选择“**下一步**”。

6. 选择“创建” > “云存储”。 

    ![](./media/azure-stack-network-howto-backup-commvault/commcell-storage-add-storage-device.png)

7. 选择云服务提供商。 在此过程中，我们将使用位于不同位置的另一个 Azure Stack Hub。 选择“Microsoft Azure 存储”。

8. 选择你的 Commvault VM 作为 MediaAgent。

9. 输入存储帐户的访问信息。 可在此处找到有关设置 Azure 存储帐户的说明。 访问信息：

    -  **服务宿主**：从资源中的 Blob 容器属性获取 URL 的名称。 例如，我的 URL 是 https:\//backuptest.blob.chinaeast.stackpoc.com/mybackups，我在服务主机中使用的是 blob.chinaeast.stackpoc.com。
    
    -   **帐户名称**：使用存储帐户名称。 可以在存储资源的“访问密钥”边栏选项卡中找到此信息。
    
    -   **访问密钥**：从存储资源的“访问密钥”边栏选项卡中获取访问密钥。
    
    -   **容器**：容器的名称。 在本例中为 mybackups。
    
    -   **存储类**：保留为用户容器的默认存储类。

10. 根据[创建 Azure Stack Hub 客户端](https://documentation.commvault.com/commvault/v11_sp13/article?p=86495.htm)中的说明创建 Azure Stack Hub 客户端

    ![](./media/azure-stack-network-howto-backup-commvault/commcell-ceate-client.png)

11. 选择要保护的 VM 或资源组并附加备份策略。

12. 根据恢复 RPO 要求配置备份计划。

13. 执行首次完整备份。

## <a name="configure-commvault-live-sync"></a>配置 Commvault Live Sync 

有两个选项可用。 可以选择将主要备份副本中的更改复制到恢复 VM，或者将次要副本中的更改复制到恢复 VM。 从备份集复制可避免对源计算机造成读取 IO 影响。

1. 在配置 Live Sync 期间，需要提供源 Azure Stack Hub（虚拟服务器代理）和目标 Azure Stack Hub 的详细信息。

2. 有关 Commvault Live Sync 的配置步骤，请参阅 [Azure Stack Hub 的 Live Sync 复制](https://documentation.commvault.com/commvault/v11_sp13/article?p=94386.htm)。

    ![](./media/azure-stack-network-howto-backup-commvault/live-sync-1.png)
 
3. 在配置 Live Sync 期间，需要提供目标 Azure Stack Hub 和虚拟服务器代理的详细信息。

    ![](./media/azure-stack-network-howto-backup-commvault/live-sync-2.png)

4. 继续配置，并添加目标存储帐户（用于托管副本磁盘）、资源组（用于放置副本 VM），以及要附加到副本 VM 的名称。

    ![](./media/azure-stack-network-howto-backup-commvault/live-sync-3.png)

5. 还可以选择每个 VM 旁边的“配置”，来更改 VM 大小和配置网络设置。

6. 设置复制到目标 Azure Stack Hub 的频率

    ![](./media/azure-stack-network-howto-backup-commvault/live-sync-5.png)

7. 检查设置并保存配置。 然后，系统将创建恢复环境，复制将按所选的间隔开始进行。


## <a name="set-up-failover-behavior-using-live-sync"></a>使用 Live Sync 设置故障转移行为

使用 Commvault Live Sync 可将计算机从一个 Azure Stack Hub 故障转移到另一个 Azure Stack Hub，并可以进行故障回复，以恢复原始 Azure Stack Hub 上的操作。 该工作流会自动完成，并会记录日志。

![](./media/azure-stack-network-howto-backup-commvault/back-up-live-sync-panel.png)

选择要故障转移到恢复 Azure Stack Hub 的 VM，然后选择计划性或非计划故障转移。 计划性故障转移适用于有时间正常关闭生产环境，然后在恢复站点中恢复操作的情况。 计划性故障转移会关闭生产 VM，将最终更改复制到恢复站点，使包含最新数据的恢复 VM 联机，并应用配置 Live Sync 期间指定的 VM 大小和网络配置。 非计划性故障转移会尝试关闭生产 VM，但如果生产环境不可用，则故障转移会继续进行。它只会使恢复 VM 联机，并对 VM 应用最后收到的复制数据集以及所选的大小和网络配置。 下图演示了非计划性故障转移，其中，Commvault Live Sync 已使恢复 VM 联机。

![](./media/azure-stack-network-howto-backup-commvault/unplanned-failover.png)

![](./media/azure-stack-network-howto-backup-commvault/fail-over-2.png)

![](./media/azure-stack-network-howto-backup-commvault/fail-over-3.png)

## <a name="next-steps"></a>后续步骤

[Azure Stack Hub 网络的差异和注意事项](azure-stack-network-differences.md)  