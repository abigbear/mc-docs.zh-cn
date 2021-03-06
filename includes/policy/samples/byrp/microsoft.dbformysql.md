---
author: WenJason
ms.service: azure-policy
ms.topic: include
origin.date: 05/05/2020
ms.date: 06/01/2020
ms.author: v-jay
ms.custom: generated
ms.openlocfilehash: e782f1a07b419039767fb639d09cf4fc09828ff5
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84200063"
---
|名称 |说明 |效果 |版本 |GitHub |
|---|---|---|---|---|
|[应为 MySQL 数据库服务器启用“强制 SSL 连接”](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe802a67a-daf5-4436-9ea6-f6d821dd0c5d) |此策略审核不强制 SSL 连接的任何 MySQL 服务器。 Azure Database for MySQL 支持使用安全套接字层 (SSL) 将 Azure Database for MySQL 服务器连接到客户端应用程序。 通过在数据库服务器与客户端应用程序之间强制实施 SSL 连接，可以加密服务器与应用程序之间的数据流，有助于防止“中间人”攻击。 |Audit、Disabled |1.0.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MySQL_EnableSSL_Audit.json)。 |
|[应为 Azure Database for MySQL 启用异地冗余备份](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F82339799-d096-41ae-8538-b108becf0970) |此策略将审核未启用异地冗余备份的任何 Azure Database for MySQL。 |Audit、Disabled |1.0.0 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/GeoRedundant_DBForMySQL_Audit.json)。 |
|[MySQL 服务器应使用虚拟网络服务终结点](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3375856c-3824-4e0e-ae6a-79e011dd4c47) |此策略审核未配置为使用虚拟网络服务终结点的 MySQL 服务器。 有关更多详细信息，请访问 [https://aka.ms/mysqlvnet](https://aka.ms/mysqlvnet)。 |AuditIfNotExists、Disabled |1.0.1 |[链接](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MySQL_VirtualNetworkServiceEndpoint_Audit.json)。 |
