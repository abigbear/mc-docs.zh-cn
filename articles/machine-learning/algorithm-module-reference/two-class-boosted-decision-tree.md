---
title: 双类提升决策树：模块参考
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 机器学习中的“双类提升决策树”模块，以便根据提升决策树算法创建机器学习模型。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: v-yiso
origin.date: 08/24/2020
ms.date: 06/22/2020
ms.openlocfilehash: 65222d446e84e8e7c600edc1c3a96f7ed9365ac7
ms.sourcegitcommit: 78c71698daffee3a6b316e794f5bdcf6d160f326
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2020
ms.locfileid: "90020863"
---
# <a name="two-class-boosted-decision-tree-module"></a>“双类提升决策树”模块

本文介绍了 Azure 机器学习设计器（预览版）中的一个模块。

使用此模块，可以根据提升决策树算法创建机器学习模型。 

提升决策树是一种集成学习方法，在此方法中，第二个树将针对第一个树的误差进行纠正，第三个树将针对第一个和第二个树的误差进行纠正，依此类推。  预测基于共同构成了预测的树的整个集成。
  
通常，当配置正确时，提升决策树是可在各种机器学习任务中获得最佳性能的最简单方法。 但是，它们也是占用大量内存的学习器之一，并且当前实现将所有内容都保存在内存中。 因此，提升决策树模型可能无法处理某些线性学习器可以处理的大型数据集。

此模块基于 LightGBM 算法。

## <a name="how-to-configure"></a>配置方式

此模块创建一个未训练的分类模型。 由于分类是一种监督式学习方法，所以，若要训练模型，你需要一个“带标记的数据集”，其中包含一个标签列，该列在所有行中都有一个值。

你可以使用[训练模型](././train-model.md)来训练这种类型的模型。 

1.  在 Azure 机器学习中，将**提升决策树**模块添加到你的管道。
  
2.  通过设置“创建训练程序模式”选项，指定要如何对模型进行训练。
  
    + “单个参数”****：如果你知道自己想要如何配置模型，可以提供一组特定的值作为参数。
  
    + **参数范围**：如果不确定最佳参数，可以使用[优化模型超参数](tune-model-hyperparameters.md)模块来找到最佳参数。 你提供某个值范围，然后训练程序就会循环访问多个设置组合，以确定可产生最佳结果的值组合。
  
3.  对于“每个树的最大叶数”，请指定可在任何树中创建的终端节点（叶）的最大数目。
  
     如果增大此值，则可能会增加树的大小并获得更好的精度，但风险是过度拟合和更长的训练时间。
  
4.  对于“每个叶节点的最少样本数”，指定在树中创建任何终端节点（叶）所需的事例数。  
  
     通过增加此值，可以增加创建新规则的阈值。 例如，使用默认值 1 时，即使是单个事例也可以导致创建新规则。 如果将值增加到 5，则训练数据将必须包含至少五个满足相同条件的事例。
  
5.  对于“学习速率”，请键入一个介于 0 和 1 之间的数字，用以定义学习时的步幅。  
  
     学习速率决定了学习器收敛于最优解的速度。 如果步幅太大，则可能会越过最优解。 如果步幅太小，则训练将花费更长的时间来收敛于最优解。
  
6.  对于“构造的树数”，请指定要在集成中创建的决策树的总数。 通过创建更多决策树，你可能会获得更好的覆盖范围，但训练时间将会增加。
  
     此值还控制对训练后的模型进行可视化时显示的树的数量。 如果希望查看或打印单个树，请将此值设置为 1。 但是，如果这样做，只会生成一个树（该树采用初始的参数集），不会执行进一步的迭代。
  
7.  对于“随机数种子”，可以键入非负整数作为随机种子值。 指定种子可以确保具有相同数据和参数的运行之间的可再现性。  
  
     随机种子默认设置为 0，这意味着将从系统时钟获取初始种子值。  使用随机种子的后续运行可能会产生不同的结果。
  

9. 训练模型：

    + 如果将“创建训练程序模式”设置为“单个参数”，请连接带标记的数据集和[训练模型](train-model.md)模块 。  
  
    + 如果将“创建训练程序模式”设置为“参数范围”，请连接带标记的数据集并使用[优化模型超参数](tune-model-hyperparameters.md)来训练模型 。  
  
    > [!NOTE]
    > 
    > 如果将参数范围传递给[训练模型](train-model.md)，则它只使用单个参数列表中的默认值。  
    > 
    > 如果将一组参数值传递给[优化模型超参数](tune-model-hyperparameters.md)模块，则当它期望每个参数有一系列设置时，它会忽略这些值，并为学习器使用默认值。  
    > 
    > 如果选择“参数范围”选项并为任何参数输入单个值，则整个整理过程中都会使用你指定的单个值，即使其他参数的值发生一系列更改。  
   
## <a name="results"></a>结果

在训练完成后：

+ 若要保存已训练模型的快照，请选择“训练模型”模块右侧面板中的“输出”选项卡。 选择“注册数据集”图标将模型保存为可重用模块。

+ 若要使用模型进行评分，请向管道中添加**评分模型**模块。


## <a name="next-steps"></a>后续步骤

请参阅 Azure 机器学习的[可用模块集](module-reference.md)。 