---
title: 在 Azure AD 权利管理中自我评审访问包
description: 了解如何在 Azure Active Directory 访问评审（预览版）中评审权利管理访问包的用户访问权限。
services: active-directory
documentationCenter: ''
author: ajburnle
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 08/25/2020
ms.author: v-junlch
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82e625992e07e8c39a6e586660a5b439cd32516d
ms.sourcegitcommit: b5ea35dcd86ff81a003ac9a7a2c6f373204d111d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2020
ms.locfileid: "88947392"
---
# <a name="self-review-of-an-access-package-in-azure-ad-entitlement-management"></a>在 Azure AD 权利管理中自我评审访问包

Azure AD 权利管理简化了企业管理组、应用程序和 SharePoint 站点访问权限的方式。 本文介绍用户如何对分配的访问包进行自我评审。

## <a name="open-the-access-review"></a>打开访问评审

若要进行访问评审，必须先打开访问评审。 请通过以下过程找到并打开访问评审：

1. 你可能会从 Microsoft 收到一封要求你进行访问评审的电子邮件。 请找到该电子邮件，打开访问评审。 下面是请求进行访问评审的电子邮件的示例： 
    
    ![访问评审自我评审者电子邮件](./media/entitlement-management-access-reviews-review-access/self-review-reviewer-email.png)

1. 单击“评审访问权限”链接。

1. 如果没有收到电子邮件，你也可以直接访问 https://myaccess.microsoft.com 来查找待处理的访问评审。  （对于 US Government 版，请改用 `https://myaccess.microsoft.us`。）

1. 单击左侧导航栏上的“访问评审”，查看分配给你的待处理访问评审的列表。


1.  单击要开始的评审。

## <a name="perform-the-access-review"></a>执行访问评审

打开访问评审以后，即可看到自己的访问权限。 请通过以下过程进行访问评审：

1.  确定是否仍需访问该访问包。 例如，你正在处理的项目未完成，因此仍需访问权限以继续处理该项目。

1.  单击“是”保留访问权限，或者单击“否”删除访问权限。
    >[!NOTE]
    >如果你指出不再需要访问权限，系统不会立即将你从访问包中删除。 当评审结束或管理员停止评审时，系统会将你从访问包中删除。

1.  如果单击“是”，你可能需要在“原因”框中说明原因。 

1.  单击“提交”  。

如果你改变主意，决定在评审结束之前更改响应，则可返回到评审。

## <a name="next-steps"></a>后续步骤

- [评审对访问包的访问权限](entitlement-management-access-reviews-review-access.md) 

