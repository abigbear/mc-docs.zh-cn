---
title: 用于在 Azure Cosmos DB 表 API 中创建表（具有自动缩放功能）的 PowerShell 脚本
description: 用于在 Azure Cosmos DB 表 API 中创建表（具有自动缩放功能）的 PowerShell 脚本
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: sample
origin.date: 07/30/2020
author: rockboyfor
ms.date: 08/17/2020
ms.testscope: no
ms.testdate: 08/17/2020
ms.author: v-yeche
ms.openlocfilehash: 30d96431cf4bf68f498454a88c19a303a02c3db9
ms.sourcegitcommit: 84606cd16dd026fd66c1ac4afbc89906de0709ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88223557"
---
<!--Verified successfully-->
# <a name="create-a-table-with-autoscale-for-azure-cosmos-db---table-api"></a>为 Azure Cosmos DB 创建表（具有自动缩放功能）- 表 API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

```powershell
# Reference: Az.CosmosDB | https://docs.microsoft.com/powershell/module/az.cosmosdb
# --------------------------------------------------
# Purpose
# Create Cosmos Table API account and a Table with autoscale throughput
# --------------------------------------------------
Function New-RandomString{Param ([Int]$Length = 10) return $(-join ((97..122) + (48..57) | Get-Random -Count $Length | ForEach-Object {[char]$_}))}
# --------------------------------------------------
$uniqueId = New-RandomString -Length 7 # Random alphanumeric string for unique resource names
$apiKind = "Table"
# --------------------------------------------------
# Variables - ***** SUBSTITUTE YOUR VALUES *****
$locations = @("China East", "China North") # Regions ordered by failover priority
$resourceGroupName = "mjbArmTest"#"myResourceGroup" # Resource Group must already exist
$accountName = "cosmos-$uniqueId" # Must be all lower case
$consistencyLevel = "Session"
$tableName = "myTable"
$autoscaleMaxThroughput = 4000 #minimum = 4000
# --------------------------------------------------
Write-Host "Creating account $accountName"
$account = New-AzCosmosDBAccount -ResourceGroupName $resourceGroupName `
    -Location $locations -Name $accountName -ApiKind $apiKind `
    -DefaultConsistencyLevel $consistencyLevel `
    -EnableAutomaticFailover:$true

Write-Host "Creating Table $tableName"
$table = New-AzCosmosDBTable -ParentObject $account `
    -Name $tableName -AutoscaleMaxThroughput $autoscaleMaxThroughput

```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| Command | 注释 |
|---|---|
|**Azure Cosmos DB**| |
| [New-AzCosmosDBAccount](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbaccount) | 创建 Cosmos DB 帐户。 |
| [New-AzCosmosDBTable](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbtable) | 创建表 API 表。 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

可以在 [Azure Cosmos DB PowerShell 脚本](../../../powershell-samples.md)中找到其他 Azure Cosmos DB PowerShell 脚本示例。

<!-- Update_Description: new article about autoscale -->
<!--NEW.date: 08/17/2020-->