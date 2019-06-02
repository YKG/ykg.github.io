---
title: TiDB学习笔记2
date: 2019-06-01 22:58:00
tags: [TiDB]
---

看TiDB的线上课程，做一些摘录。

### PCTA-1001 TiDB架构概论

- 讲师就是那三篇文章的作者
- MySQL、PgSql对于大数据量的处理是要做分库分表，可用性上是Master/Slave模型，比起分布式系统来说，确实不是先天支持很多特性的
  > 我也在想那以后自己用的时候，是不是也优先使用TiDB，因为很强大，何必用上个时代的产品呢
- 事务模型（Google Percolator）回头要查下资料了解下
- 两阶段提交来保证事务，实现了分布式，集中点只在时间戳分配中心
- 乐观事务模型：只在commit时进行冲突检查
- 隔离级别：Snapshot isolation，具体是什么意思？
- PD: Placement Driver，本身也是分布式的，3副本，Raft
- Row和Index的具体存储，注意的是Index的value存储得是RowId
  > Row:   {key: TableId + RowId, value: Row Value}  
  > Index: {key: TableId + IndexId + Index-Column-Values, value: RowId}
- KV的的k、v都是按照byte数组存放
- 尽可能将计算下推到KV层
- Syncer，同步MySql的binlog到TiDB，假装自己是MySQL slave
- 什么是binlog？
- 备份/恢复工具
  > mydumper  
  > Loader支持断点续传  
  > TiDB-Lighting直接生成TiKV file，1T数据6小时（越过SQL层？）

### PCTA-1002 SQL基础知识

- 数据类型，目前没有支持空间类型，字符集当前只支持utf8mb4
  > Numeric
  > Date & Time
  > String
  > JSON
- char最长255，select出来后会把结尾填充的空格去掉
- varchar最长65535
- binary插入时，末尾会用0x00填充，select出来不会去掉末尾的0x00
- auto_increment不保障连续分配，只保证自增和唯一性
- TiDB单表最多支持512列
- index_col_name最长支持3072字节
- Analyze Table收集表的统计信息
- 支持explain，但返回结果和MySQL的不太一样
- Index Hint：use index，force index，ignore index

