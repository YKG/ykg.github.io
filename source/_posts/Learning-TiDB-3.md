---
title: TiDB学习笔记3
date: 2019-06-02 22:12:47
tags: [TiDB]
---

### PCTA-1003 TiKV集群基础知识

- 一般数据库是B+树数据结构
- RocksDB的数据结构：LSM，问题：写放大
- TiDB自己的Titan存储引擎做了一些优化（基于LSM?）,不过看PCTA-4001黄东旭讲的提到微软的一个东西，似乎就是这个算法
- Raft/Multi-Raft

### PCTA-1004 TiDB开源社区及文档介绍

- 这个视频是唯一一个女声的，没有杂音，语言能力很好，没有任何磕绊，质量很高

### PCTA-4001 分布式数据库概论

- 上面提到的就是WiscKey（优化的LSM）
- Google的三大论文应该读一下
- Hadoop的生态原来那么大
- Codis是Redis的一个集群版本，原来黄就是它的作者
- Kubernetes会变成一个集群的分布式操作系统

### PCTA-2001 TiDB安装部署及集群管理

- 配置要求挺高的，想自己本地跑似乎不太可能
- Prometheus原来就是普罗米修斯的英文
- Ansible

### PCTA-2005 TiDB业务开发最佳实践

- CURSOR STABILITY
- Write Skew

### PCTA-3001 TiDB-Lighting介绍

- Kenny Chan应该是粤语区人

### PCTA-3002 TiDB-Data Migration介绍

- 学程湖南人，湖南话有意思


