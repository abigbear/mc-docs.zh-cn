---
title: 开发最佳做法
description: 开发 Synapse SQL 池解决方案时应了解的建议和最佳做法。
services: synapse-analytics
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
origin.date: 09/04/2018
ms.date: 05/11/2020
ms.author: v-jay
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 53feb39dc64e66237f1b3026a59f31c97e2f3c09
ms.sourcegitcommit: f8d6fa25642171d406a1a6ad6e72159810187933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82198522"
---
# <a name="development-best-practices-for-synapse-sql-pool"></a>Synapse SQL 池开发最佳做法

本文介绍在开发 SQL 池解决方案时的指导和最佳做法。

## <a name="tune-query-performance-with-new-product-enhancements"></a>使用新的产品增强功能优化查询性能

- [通过具体化视图进行性能优化](performance-tuning-materialized-views.md)
- [通过有序聚集列存储索引进行性能优化](performance-tuning-ordered-cci.md)
- [通过结果集缓存进行性能优化](performance-tuning-result-set-caching.md)

## <a name="reduce-cost-with-pause-and-scale"></a>使用暂停和缩放来降低成本

若要了解如何通过暂停和缩放来降低成本，请参阅[管理计算](sql-data-warehouse-manage-compute-overview.md)一文。

## <a name="maintain-statistics"></a>维护统计信息

可将 SQL 池配置为自动检测和创建有关列的统计信息。  优化工具创建的查询计划质量以可用的统计信息为限。  

建议为数据库启用 AUTO_CREATE_STATISTICS，并使统计信息每日更新或者在每次加载后更新，确保查询中使用的有关列的统计信息始终保持最新。

如果你发现更新所有统计信息需要太长的时间，可以尝试更精心地选择哪些列的统计信息需要频繁更新。 例如，可能想要更新每天都要添加新值的日期列。

> [!TIP]
> 对于涉及联接的列、WHERE 子句中使用的列、在 GROUP BY 中找到的列，更新其统计信息可以获得最大效益。

另请参阅[管理表统计信息](sql-data-warehouse-tables-statistics.md)、[CREATE STATISTICS](sql-data-warehouse-tables-statistics.md) 和 [UPDATE STATISTICS](sql-data-warehouse-tables-statistics.md#update-statistics)。

## <a name="hash-distribute-large-tables"></a>哈希分布大型表

默认情况下，表是以轮循机制分布的。  此设计可让用户更容易开始创建表，而不必确定应该如何分布其表。  

轮循机制表在某些工作负荷中执行良好，但大多数情况下，选择分布列的执行性能将更好。  按列分布表的性能远远高于轮循机制表的最常见例子是联接两个大型事实表。  

例如，如果有一个依 order_id 分布的订单表，以及一个也是依 order_id 分布的事务表，如果将订单数据联接到事务表上的 order_id，此查询将变成传递查询，也就是数据移动操作将被消除。  减少步骤意味着加快查询速度。  更少的数据移动也会让查询更快。  

加载分布的表时，请确保传入数据的分布键没有排序，因为这会拖慢加载速度。  以下文章进一步详述了如何选择分布列来提升性能，以及如何在 CREATE TABLES 语句的 WITH 子句中定义分布式表。

另请参阅[表概述](sql-data-warehouse-tables-overview.md)、[表分布](sql-data-warehouse-tables-distribute.md)、[选择表分布](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/)、[CREATE TABLE](sql-data-warehouse-tables-overview.md) 和 [CREATE TABLE AS SELECT](sql-data-warehouse-develop-ctas.md)

## <a name="do-not-over-partition"></a>不要过度分区

尽管数据分区可以让数据维护变得有效率（通过分区切换或优化扫描将分区消除），太多的分区将让查询变慢。  

在 SQL Server 上运行良好的高数据粒度分区策略经常无法在 SQL 池中正常使用。  如果每个分区的行数少于 1 百万，太多分区还会降低聚集列存储索引的效率。  

请记住，SQL 池在幕后将数据分区成 60 个数据库，因此如果创建包含 100 个分区的表，实际上会生成 6000 个分区。  每个工作负荷都不同，因此最佳建议是尝试不同的分区，找出最适合工作负荷的分区。  

> [!TIP]
> 请考虑使用比在 SQL Server 上运行良好的数据粒度更低的粒度。  例如，考虑使用每周或每月分区，而不是每日分区。

另请参阅[表分区](sql-data-warehouse-tables-partition.md)。

## <a name="minimize-transaction-sizes"></a>最小化事务大小

在事务中运行的 INSERT、UPDATE、DELETE 语句，失败时必须回滚。  为了将长时间回滚的可能性降到最低，请尽可能将事务大小最小化。  这可以通过将 INSERT、UPDATE、DELETE 语句分成小部分来达成。  

例如，如果预期 INSERT 需要 1 小时，可能的话，将 INSERT 分成 4 个部分，每个部分运行 15 分钟。  使用特殊的最低限度日志记录方案，像是 CTAS、TRUNCATE、DROP TABLE 或 INSERT 空表，来降低回滚的风险。  

另一个消除回滚的作法是使用“仅元数据”操作（像是分区切换）进行数据管理。  例如，不要运行 DELETE 语句来删除表中所有 order_date 为 2001 年 10 月的行，而是将数据每月分区后，再从另一个表将有空分区之数据的分区调动出来（请参阅 ALTER TABLE 示例）。  

针对未分区的表，请考虑使用 CTAS 将想要保留的数据写入表中，而不是使用 DELETE。  如果 CTAS 需要的时间一样长，则较安全的操作，是在它具有极小事务记录的条件下运行它，且必要时可以快速地取消。

另请参阅[了解事务](sql-data-warehouse-develop-transactions.md)、[优化事务](sql-data-warehouse-develop-best-practices-transactions.md)、[表分区](sql-data-warehouse-tables-partition.md)、[TRUNCATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/truncate-table-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)、[ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) 和 [Create table as select (CTAS)](sql-data-warehouse-develop-ctas.md)。

## <a name="use-the-smallest-possible-column-size"></a>使用最小可能的列大小

在定义 DDL 时，使用可支持数据的最小数据类型，能够改善查询性能。  此方法对 CHAR 和 VARCHAR 列特别重要。  

如果列中最长的值是 25 个字符，请将列定义为 VARCHAR(25)。  避免将所有字符列定义为较大的默认长度。  此外，将列定义为 VARCHAR（当它只需要这样的大小时）而非 NVARCHAR。

另请参阅[表概述](sql-data-warehouse-tables-overview.md)、[表数据类型](sql-data-warehouse-tables-data-types.md)和 [CREATE TABLE](sql-data-warehouse-tables-overview.md)。

## <a name="optimize-clustered-columnstore-tables"></a>优化聚集列存储表

聚集列存储索引是将数据存储在 SQL 池中最有效率的方式之一。  默认情况下，SQL 池中的表会创建为聚集列存储。  

> [!NOTE]
> 为了让列存储表的查询实现最佳性能，良好的分段质量很重要。  

当行在内存不足的状态下写入列存储表时，列存储分段质量可能降低。  压缩行组中的行数可以测量分段质量。  

有关检测和改善聚集列存储表分段质量的分步说明，请参阅[表索引](sql-data-warehouse-tables-index.md)一文中的[列存储索引质量不佳的原因](sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality)。  

由于高质量列存储段很重要，因此可以考虑使用用来加载数据的中型或大型资源类中的用户 ID。 使用较低的[数据仓库单位](what-is-a-data-warehouse-unit-dwu-cdwu.md)值意味着需要向加载用户分配较大的资源类。

列存储表通常要等到每个表有超过 1 百万个行之后才会数据推送到压缩的列存储区段，并且每个 SQL 池表会分区成 60 个表。因此，一般情况下，列存储表对于查询没有好处，除非表有超过 6 千万行。  

小于 6 千万列的表使用列存储索引似乎不太合理，  但也无伤大雅。  

此外，如果要进行数据分区，则要考虑的是每个分区必须有 1 百万个行，使用聚集列存储索引才有益。  如果表有 100 个分区，则它至少必须拥有 60 亿个行才会受益于聚集列存储（60 个分布区 *100 个分区* 1 百万行）。  

如果表在本示例中并没有 60 亿个行，请减少分区数目，或考虑改用堆表。  使用辅助索引配合堆表而不是列存储表，也可能是值得进行的实验，看看是否可以获得较佳的性能。

> [!TIP]
> 查询列存储表时，如果只选择需要的列，查询运行速度更快。  

另请参阅[表索引](sql-data-warehouse-tables-index.md)、[列存储索引指南](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)和[重新生成列存储索引](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality)。

## <a name="next-steps"></a>后续步骤

如果在本文中没有找到所需内容，可尝试使用本页左侧的“搜索文档”来搜索所有 Azure Synapse 文档。  

在 [Azure Synapse 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home)中，可以向其他用户和 Azure Synapse 产品小组提问。  我们会主动观察此论坛，确保用户的问题获得其他用户或我们的回答。  


