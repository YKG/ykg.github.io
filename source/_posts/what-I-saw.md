---
title: 阅读清单
date: 2019-06-01 23:39:16
tags: [清单]
---

### 2019-06-01

- RocksDB是基于Google的Level DB的
- Level DB两位作者，第二位是Jeff Dean，查了Jeff的知乎相关回答，简直神话了
- CNAME是把一个域名映射到另一个域名上，目的是只要该一个A记录，所以CNAME都一起更新了，A记录的那条域名相当于一个中间层
- 看到一个light-4j，号称性能44x于spring-boot-tomcat，但看知乎的评价，其实减少很多特性谁都可以搞一个，看来sprint-boot还是值得使用的
- Ubuntu的官方支持是Canonical公司，是有Remote职位的
- Manjaro现在已经是第一流行了，以前是第二位，新出来一个MX Linux在最近3个月榜单上是第一，但看评价说是刷榜刷上去的
- Ansible是TiDB指定的生产环境部署工具，查了下，同等地位的也有puppet，Tradeshft当年在用，对这些不了解
- 看了TiDB线上课程的前两节

### 2019-06-02

- 得知姥爷去世
- 把TiDB的线上视频全部看完
- 看了《普京访谈录》第一集
- 《李自然说》更新了，加了微信，进了群，写了段介绍给他

### 2019-06-03

- 用docker-composer在本地开了tidb，用mysql连了下，尝试了snapshot
- 看tikv的文档，提到Raft实现，看到在raft.github.io上的链接，进了tiancaiamao的blog，打算联系下投简历的，还是再等等吧
- 看了fuyang也在tikv上提交了3个PR
- 晚上看了下rust的入门资料，又查了王垠的批评
- 王垠之前只是说去了上海，没想到是去了Intel，看到知乎的问题才知道

### 2019-06-04

- etcd也是一个分布式kv存储
- Beanote这个Chrome插件可以给网页加注释
- 买了6月8日北京-正定机场-贵阳的车票和机票，如今还是携程做得最好，去哪儿不行了
- 新注册了GCP账号，只够一台机器8c52G的，编译TiKV
- 和Dazhang聊了会儿，作业群、猫池、付费问卷
- 提交第一个TiKV的PR
- 指导新疆执行遇到的小兄弟安装Win10系统
- 看到了ice1000.org的Blog，原来是00后。。
- 飘飘熊原来是位加拿大妹子

### 2019-06-05

- 中午给`pingcap/docs`提交了3个PR，截止23:12但没有反馈
- 发现昨天提交的PR检查gdb是无效的，很慌，赶紧上了一个修复
- TiKV编译很耗时间，差不多半小时，看能不能加速

### 2019-06-06

- 看`brson`给TiKV提交的有关`Build-Time`的issue和PR，他还是知道很多的，诸如rust compiler pipeline和LTO等优化

### 2019-06-07

- 收拾行李

### 2019-06-08

- 来贵阳
- 在正定机场，横向步行电梯维修，侧面没有围栏，没注意脚下，踏空，左膝下部擦伤

### 2019-06-09

- 讨论接下来工作计划

### 2019-06-10

- 做了下高考数学题维纳斯黄金分割人身高
- 读史记的货殖列传
- 尝试直接使用TiKV的RawKV，一直到晚上才弄通，文档还是不够清楚

### 2019-06-11

- 学习Rust
- 尝试微信小程序里使用canvas生成海报，试了最基础图片生成

### 2019-06-23

- 配置maven镜像，163
- 查了springboot启动时间加速相关的资料
- docker版mysql的root密码找回
- alpine linux
- Tini
- mysql grant命令
- jdbcTemplate/MyBatis/Spring Data JPA对比

### 2019-06-24

- 继续阅读Rust语言中文入门
- 小程序海报生成服务端实现（[laravel-miniprogram-poster][1], [poster-generater][2]）
- Headless Chrome, 这就是PhantomJS停止更新的原因
- Puppeteer, Headless Chrome提供的高级操作库，有赞本月13号发布[一篇文章][3]讲他们正在用这个做



[1]: https://learnku.com/articles/18418
[2]: https://blog.janguly.com/poster
[3]: https://developers.weixin.qq.com/community/develop/article/doc/00022cd6eb09a8933bb8b8f3d56013

