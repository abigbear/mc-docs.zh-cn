---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 43d3484f18f0c5c6ff565aa4e1ef60baa2f25e27
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63822449"
---
在此步骤中，在故障转移群集管理器和 SQL Server Management Studio 中手动创建可用性组侦听器。

1. 从托管主副本的节点打开故障转移群集管理器。

2. 选择“网络”节点，并记下群集网络名称。  此名称用于 PowerShell 脚本的 $ClusterNetworkName 变量中。

3. 展开群集名称，并单击“角色”。 

4. 在“角色”窗格中，右键单击可用性组名称，并选择“添加资源” > “客户端接入点”。

    ![为可用性组添加客户端接入点](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. 在“名称”框中，为此新的侦听器创建一个名称，单击“下一步”两次，并单击“完成”。     
    不要在此时使侦听器或资源联机。

6. 单击“资源”选项卡，并展开刚创建的客户端接入点。  
    会显示群集中每个群集网络的 IP 地址资源。 如果这是仅适用于 Azure 的解决方案，则只显示一个 IP 地址资源。

7. 执行下列任一操作：

   * 若要配置混合解决方案，请执行以下操作：

        a. 右键单击与本地子网对应的 IP 地址资源，并选择“属性”。  记下 IP 地址名称和网络名称。

        b. 选择“静态 IP 地址”，分配未使用的 IP 地址，并单击“确定”。  

   * 若要配置仅限 Azure 的解决方案，请执行以下操作：

        a. 右键单击与 Azure 子网对应的 IP 地址资源，并选择“属性”。 

       > [!NOTE]
       > 如果随后侦听器因 DHCP 选择的 IP 地址冲突而无法联机，可以在此属性窗口中配置有效的静态 IP 地址。
       > 
       > 

       b. 在同一个“IP 地址”属性窗口中，更改“ IP 地址名称”。    
        该名称用于 PowerShell 脚本的 $IPResourceName 变量中。 如果解决方案跨多个 Azure 虚拟网络，请对每个 IP 资源重复此步骤。
        
<!-- Update_Description: update meta properties -->