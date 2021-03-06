---
title: minif()（聚合函数）- Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 minif()（聚合函数）。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 08/06/2020
ms.openlocfilehash: aaa0e52497fa555dea7b2443692439ea676f28e2
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841583"
---
# <a name="minif-aggregation-function"></a>minif()（聚合函数）

返回其谓词的计算结果为 `true` 的组中的最小值。

* 只能在 [summarize](summarizeoperator.md) 内的聚合上下文中使用

另请参阅 - [min()](min-aggfunction.md) 函数，该函数在不使用谓词表达式的情况下返回组中的最小值。

**语法**

`summarize` `minif(`*Expr*`,`*Predicate*`)`

**参数**

* Expr：用于聚合计算的表达式。
* Predicate：在谓词为 true 的情况下，将检查 Expr 的计算值是否为最小值。

**返回**

其谓词计算结果为 `true` 的组中 Expr 的最小值。

**示例**

```kusto
range x from 1 to 100 step 1
| summarize minif(x, x > 50)
```

|minif_x|
|---|
|51|