# 基于成本的优化

> 原文:[https://www.geeksforgeeks.org/cost-based-optimization/](https://www.geeksforgeeks.org/cost-based-optimization/)

[查询优化](https://www.geeksforgeeks.org/query-optimization-in-relational-algebra/)是选择执行 [SQL](https://www.geeksforgeeks.org/sql-tutorial/) 语句的最高效或最有利类型的过程。查询优化是一门应用规则重写查询中调用的操作符树并产生最佳计划的科学艺术。如果一个计划在最少的时间内或者通过使用最少的空间返回答案，那么这个计划被认为是最优的。

**基于成本的优化:**
对于给定的查询和环境，优化器以数字形式分配与可能的计划的每个步骤相关的成本，然后一起找到这些值，以获得计划或可能策略的成本估计。在计算了所有可能的计划的成本后，优化器试图选择一个成本估计可能最低的计划。因此，优化器有时被称为基于成本的优化器。以下是基于成本的优化的一些特性-

1.  基于成本的优化基于要优化的查询的成本。
2.  查询可以基于索引的值、可用的排序方法、约束等使用许多路径。
3.  查询优化的目的是以算法的形式，以尽可能低的成本选择实现查询的最有效路径。
4.  执行算法的成本需要由查询优化器提供，以便为操作选择最合适的查询。
5.  算法的成本也取决于输入的基数。

**成本估算:**
为了估算不同可用执行计划或执行策略的成本，查询树被视为一种数据结构，包含一系列基本操作，这些操作被链接以执行查询。查询中存在的操作的成本取决于选择操作的方式，例如，形成输出的选择操作的比例。知道操作输出的预期基数也很重要。输出的基数非常重要，因为它构成了下一个操作的输入。
查询优化的成本取决于以下因素-

1.  **基数-**
    基数是通过执行查询执行计划指定的操作返回的行数。基数的估计必须是正确的，因为它高度影响执行计划的所有可能性。
2.  **选择性-**
    选择性是指选择的行数。表中任何行或数据库中任何表的选择性几乎取决于条件。条件的满足将我们带到特定行的选择性。根据情况，需要满足的条件可以是任何条件。
3.  **成本-**
    成本是指为优化系统而花费在系统上的资金量。成本的衡量完全取决于完成的工作或使用的资源数量。

第一步是使用 ANALYZE TABLE COMPUTE STATISTICS SQL 命令计算表统计信息。使用 DESCRIBE EXTENDED SQL 命令检查统计信息。

**表统计:**
表统计可以针对表、分区和列进行计算，具体如下-

1.  **表或表分区的总大小**(以字节为单位)。
2.  表或表分区的行数。
3.  **列统计**如最小值、最大值、num _ nulls、distinct_count、avg_col_len、max_col_len、直方图。

**ANALYZE TABLE COMPUTE STATISTICS SQL 命令:**
基于成本的优化使用存储在元存储中的统计数据，即使用 ANALYZE TABLE SQL 命令的外部目录-

```
ANALYZE TABLE tableIdentifier partitionSpec;
COMPUTE STATISTICS (NOSCAN | FOR COLUMNS identifierSeq);
```

根据不同的变量，ANALYZE TABLE 计算不同的统计信息，即表、分区或列的统计信息

*   分析既没有分区说明也没有 FOR COLUMNS 子句的表。
*   使用分区规范分析表(但没有 FOR COLUMNS 子句)。
*   使用 FOR COLUMNS 子句分析表(但没有分区说明)。

**description EXTENDED SQL 命令:**
使用 description EXTENDED SQL 命令可以查看表、分区或列(存储在元存储中)的统计信息-

```
(DESC | DESCRIBE) TABLE? (EXTENDED | FORMATTED);
tableIdentifier partitionSpec? describeColName;
```

**执行查询的成本构成:**
以下是执行查询的成本构成-

1.  **二级存储的访问成本-**
    这可能是搜索、读取或写入最初在二级存储(尤其是磁盘)上找到的数据块的成本。在文件中搜索记录的成本也取决于文件的访问结构类型。
2.  **内存使用成本-**
    内存使用成本可以简单地通过使用执行查询所需的内存缓冲区数量来计算。
3.  **存储成本-**
    存储成本是存储由查询的执行策略生成的任何中间文件(处理输入的结果但不完全是结果的文件)的成本。
4.  **计算成本-**
    这是执行数据缓冲区内记录上可用的内存操作的成本。搜索记录、合并记录或排序记录等操作。这也可以称为 CPU 成本。
5.  **通信成本-**
    这是与从一个地方向另一个地方发送或传送查询及其结果相关联的成本。它还包括在查询评估过程中将表和结果传输到各个站点的成本。

**基于成本的优化中的问题:**
以下是基于成本的优化中的问题-

1.  在基于成本的优化中，可以考虑的执行策略的数量并不是真正固定的。执行策略的数量可能因情况而异。
2.  有时，这个过程确实非常耗时，因为它并不总是能保证找到最佳的策略
3.  这是一个昂贵的过程。