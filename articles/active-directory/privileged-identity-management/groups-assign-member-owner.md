---
title: 为特权访问组分配符合条件的所有者和成员 - Azure Active Directory
description: 了解如何在 Azure AD Privileged Identity Management (PIM) 中分配符合条件的可分配角色组的所有者或成员。
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 08/14/2020
ms.author: v-junlch
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 06952316757a1a1a420e662f685d5bdde3ed1904
ms.sourcegitcommit: 7646936d018c4392e1c138d7e541681c4dfd9041
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/20/2020
ms.locfileid: "88648030"
---
# <a name="assign-eligibility-for-a-privileged-access-group-preview-in-privileged-identity-management"></a>在 Privileged Identity Management 中分配特权访问组（预览）的资格

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) 可以帮助你管理 Azure AD 中特权访问组分配的资格和激活。 可以为组的成员或所有者分配资格。

>[!NOTE]
>每个有资格成为特权访问组成员或所有者的用户都必须拥有 Azure AD Premium P2 许可证。 有关详细信息，请参阅[使用 Privileged Identity Management 的许可要求](subscription-requirements.md)。

## <a name="assign-an-owner-or-member-of-a-group"></a>分配组的所有者或成员

按照以下步骤操作，使用户有资格成为特权访问组的成员或所有者。

1. 在 Azure 门户中使用[特权角色管理员](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)角色权限登录到 [Privileged Identity Management](https://portal.azure.cn/)。

    有关如何授予其他管理员访问权限以管理 Privileged Identity Management 的信息，请参阅[授予其他管理员访问权限以管理 Privileged Identity Management](pim-how-to-give-access-to-pim.md)。

1. 打开“Azure AD Privileged Identity Management”。

1. 选择“特权访问组(预览)”。

1. 你可以搜索组名称，并使用“组类型”筛选列表以选择要管理的组。

    ![要管理的特权访问组的列表](./media/groups-assign-member-owner/privileged-access-list.png)

1. 在“管理”下选择“分配” 。

1. 选择“添加分配”。

    ![“新建分配”窗格](./media/groups-assign-member-owner/groups-add-assignment.png)

1. 选择要使其符合特权访问组资格的成员或所有者。

    ![“选择成员或组”窗格](./media/groups-assign-member-owner/add-assignments.png)

1. 选择“下一步”以设置成员资格或所有权持续时间。

    ![“选择成员或组”窗格](./media/groups-assign-member-owner/assignment-duration.png)

1. 在“分配类型”**** 列表中，选择“合格”**** 或“活动”****。 特权访问组提供两种不同的分配类型：

    - “合格”分配要求该角色的成员执行某个操作才能使用该角色。 操作可能包括执行多重身份验证 (MFA) 检查、提供业务理由或请求获得指定审批者的批准。

    - “活动”分配不要求成员执行任何操作便可使用该角色。 分配为“活动”的成员始终具有分配给该角色的权限。

1. 如果分配应该是永久性的（永久合格或永久分配），请选中“永久”**** 复选框。 根据组织的设置，该复选框可能不会显示或不可编辑。

1. 完成后，选择“分配”。

1. 若要创建新的角色分配，请选择“添加”****。 显示状态通知。

    ![新建分配 - 通知](./media/groups-assign-member-owner/groups-assignment-notification.png)

## <a name="update-or-remove-an-existing-role-assignment"></a>更新或删除现有的角色分配

按照以下步骤更新或删除现有的角色分配。

1. 打开“Azure AD Privileged Identity Management”。

1. 选择“特权访问组(预览)”。

1. 你可以搜索组名称，并使用“组类型”筛选列表以选择要管理的组。

    ![要管理的特权访问组的列表](./media/groups-assign-member-owner/privileged-access-list.png)

1. 在“管理”下选择“分配” 。

1. 选择要更新或删除的角色。

1. 在“合格角色”**** 或“活动角色”**** 选项卡上查找角色分配。

    ![更新或删除角色分配](./media/groups-assign-member-owner/groups-add-assignment.png)

1. 选择“更新”**** 或“删除”**** 以更新或删除角色分配。

    有关扩展角色分配的信息，请参阅[在 Privileged Identity Management 中扩展或续订 Azure 资源角色](pim-resource-roles-renew-extend.md)。

## <a name="next-steps"></a>后续步骤

- [在 Privileged Identity Management 中扩展或续订 Azure 资源角色](pim-resource-roles-renew-extend.md)
- [在 Privileged Identity Management 中配置 Azure 资源角色设置](pim-resource-roles-configure-role-settings.md)
- [在 Privileged Identity Management 中分配 Azure AD 角色](pim-how-to-add-role-to-user.md)

