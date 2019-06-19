---
layout: post
title: oracle  执行计划
no-post-nav: true
category: oracle
tags: [oracle]
keywords: 执行计划
---
# ORACLE explain  Field
## 一 各个字段的注释 

   > 字段名|字段类型|含义
   > :--|:--|:--|:--|:--
   > STATEMENT_ID |VARCHAR2(30) | 语句中所指定的最优STATEMENT_ID 参数值
   > REMARKS | VARCHAR2(80) | 与被解释规划的各步骤相关联的注释最长可达80 字节
   > OPERATION | VARCHAR2(30) |各步骤所执行内部操作的名称在某条语句所产生的第一行中该列的可能取值如下DELETE STATEMENT INSERT STATEMENT SELECT STATEMENT UPDATE STATEMENT
   > OPTIONS | VARCHAR2(30) | 对OPERATION 列中所描述操作的变种
   > BJECT_NODE | VARCHAR2(128) | 用于访问对象的数据库链接database link 的名称对于使用并行执行的本地查询该列能够描述操作中输出的次序
   > OBJECT_OWNER | VARCHAR2(30) | 对于包含有表或索引的架构schema 给出其所有者的名称
   > OBJECT_NAME | VARCHAR2(30) | 表或索引的名称
   > OBJECT_INSTANCE | INTEGER | 根据对象出现在原始original 语句中的次序所给出的相应次序编号就原始的语句文本而论其处理顺序为自左至右自外向内景象扩张view
   > OBJECT_TYPE | VARCHAR2(30) | 用于提供对象描述性信息的修饰符例如索引的NON-UNIQUE
   > OPTIMIZER | VARCHAR2(255) | 当前优化程序的模式
   > ID | INTEGER | 分配给执行规划各步骤的编号
   > PARENT_ID | INTEGER |  对ID 步骤的输出进行操作的下一个执行步骤的ID
   > POSITION | INTEGER | 对于具有相同PARENT_ID 的步骤其相应的处理次序
   > COST | INTEGER | 根据优化程序的基于开销的方法所估计出的操作开销值对于使用基于规则方法的语句该列为空该列值没有特定的测量单位它只是一个用于比较执行规划开销大小的权重值
   > CARDINALITY | INTEGER | 根据基于开销的方法对操作所访问行数的估计值
   > BYTES | INTEGER | 根据基于开销的方法对操作所访问字节的估计
   > ROWS | INTEGER | 从一个源返回的记录数，这个行源可能是一个表，一个索引，也可能是一个子查询
   > TempSpc | INTEGER | 预估操作使用临时表空间的大小 
   > TIME | DATE | 预估执行操作所需要的时间(HH:MM:SS)
## 二 执行计划的顺序
执行顺序的原则是：由上至下，从右向左 
由上至下：在执行计划中一般含有多个节点，相同级别(或并列)的节点，靠上的优先执行，靠下的后执行 
从右向左：在某个节点下还存在多个子节点，先从最靠右的子节点开始执行。

一般按缩进长度来判断，缩进最大的最先执行，如果有2行缩进一样，那么就先执行上面的。
## 三 Oracle 扫描记录的方式
  1. 全表扫描-（Full Table Scans， FTS）
  2. ROWID的扫描 - （Table Access by ROWID或rowid lookup）
  3. 索引扫描（Index Scan或index lookup）有4种类型的索引扫描：  
    　（1）  索引唯一扫描（index unique scan）  
    　（2）  索引范围扫描（index range scan）  
           在非唯一索引上都使用索引范围扫描。使用index rang scan的3种情况：   
    　　  （a） 在唯一索引列上使用了range操作符（> < <> >= <= between）  
    　　  （b） 在组合索引上，只使用部分列进行查询，导致查询出多行  
    　　  （c） 对非唯一索引列上进行的任何查询。　　  
    　（3） 索引全扫描（index full scan）  
    　（4） 索引快速扫描（index fast full scan）  
## 四 连接方式
   1. 嵌套循环 - NESTED LOOPS  
      - 最适用也是最简单的连接方式。类似于用两层循环处理两个游标，外层游标称作驱动表，Oracle检索驱动表的数据，一条一条的代入内层游标，查找满足WHERE条件的所有数据，因此内层游标表中可用索引的选择性越好，嵌套循环连接的性能就越高。
   2. 哈希连接 - HASH JOIN
      - 先将驱动表的数据按照条件字段以散列的方式放入内存，然后在内存中匹配满足条件的行。哈希连接需要有合适的内存，而且必须在CBO优化模式下，连接两表的WHERE条件有等号的情况下才可以使用。哈希连接在表的数据量较大，表中没有合适的索引可用时比嵌套循环的效率要高。
   3. 排序合并连接 - SORT MERGE JOIN