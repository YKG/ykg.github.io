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

### 2019-06-25

- 早上读了两小节Rust语言中文入门
- 使用Srping JPA，很好，省事
- 使用crontab重新处理了nas的ddns
- docker-compose up --build 可以应用新的compose.yml配置，发现原来的mysql数据都还在
- compose.yml可以配置TZ=Asia/Shanghai来让mysql使用CST时区
- spring.jpa.properties.hibernate.jdbc.time_zone=Asia/Shanghai
- 把Entity里面的`Timestamp`换成了`LocalDateTime`，这样看时间可以用CST时间


### 2019-06-26

- SBC -- Single Borad Computer

### 2019-06-27

- 把docker从入门到实践的gitbook浏览完了
- 原来bash hisotry存放在`~/.bash_history`中
- 试用FileBrowser和ownCloud，最后选用ownCloud作为赛图上传前端
- 发现nas上存在大量redir的僵尸进程，该用nginx进行端口转发（反向代理）


### 2019-06-28

- NextCloud和ownCloud的关系
- [使用NextCloud和COS搭建个人网盘][4]
- 买了2C4G6M北京3区的腾讯云，1400，3年，淘宝买的。有个2C4G5M的活动，1200，3年，都只有上海节点了。达叔建议上5M的，不多花钱。
- 测试带宽，speedtest-cli，发现腾讯云的最大下载只有20～30Mbps，上传更低。代理不如用nas的，nas的带宽是100Mbps的。
- 配置试用Azure VPN Gateway产品，Basic的只有SSTP，IKEv2没得选，选高级的配了IKEv2，通了，但带宽很差，还不如ss直接连接


### 2019-06-29

- 修复tradingview的alert列表获取
- 发现tv的create-alert失败，修复工作量很大

### 2019-06-30

- 使用puppeteer来进行tv的alert创建，但后来发现可能延迟很高，还有要切换symbol的问题
- 晚上去tfd，决定重新回到之前的发数据包道路，分析数据包差异，没有做完
- 将《两月回顾（2019.6）》写了，发了。感觉没啥价值

### 2019-07-01

- 从早上开始到16:20把Tradingview的问题修复了，更新了文档
- iperf3测试nas和bj服务器的带宽，打算先用nas作为服务器
- 查找NextCloud docker安装资料
- 发现ahk的磁盘一直是busy，像是jdb2弄的，还有WAgent什么的，磁盘avio达上千ms，找了很多工具，却不怎么会用
- 协助CR配置tv bot服务器环境

### 2019-07-02

- 看极客时间上买的课程里的磁盘IO相关部分，尝试了大量命令
- atop重的avio是误报
- nextcloud装到bj服务器上了

### 2019-07-03

- 新开Azure Korea Central节点
- 将所有服务器（除了us2）的ssh端口都改成非22

### 2019-07-05

- 了解怎么使用docker的volumes，包括named和bind mount
- bind mount型在Mac上（Docker For Mac）文件同步性能很差，甚至不如我访问远在北京的服务器的速度
- 看到有很多issue关于上面那个同步问题
- 据称mutagen比docker-sync对于这个问题处理得好
- 使用brew安装mutagen时，下载速度极慢，使用`ALL_PROXY=socks5://127.0.0.1:1086 brew ...`命令前缀解决
- 要调试php-fpm，无法抓包，Mac上没有docker0（官网文档也说了，是创建了一个虚拟机，所以没有）
- docker exec进入安装tcpdump并使用`-w`参数写入同步目录，得到的pcap文件直接用wireshark打开，免去了从container中复制到本地
- php-fpm的包不是http的，不知道有什么好的调试工具，尝试打开error_log，未实现
- 现在的php已经不需要在文件末尾写`?>`了，有点像`#!/bin/bash`只用写个开头标志
- iptables TEE可以用来镜像流量，但只能在同一个网段中，否则必须配置nexthop

### 2019-07-06

- 配置Docker + php + xdebug + PhpStorm远程调试环境，但似乎有问题，遂放弃docker，该用vagrant
- 看vagrant使用说明，原来就是vm headless，效率还是很高的
- vagrant + php + xdebug + PhpStorm远程调试OK
- SearX、[CYBER WEAPONS LAB VIDEOS][5]

### 2019-07-07

- 尝试创建自己的vagrant box，也能用，但是先遇到了ssh问题，后来又遇到无法共享`/vagrant`问题
- 把nextcloud的linux开发环境搭建了，但并不怎么顺手，隔着一个vm，创建app不知道如何调试
- 反思是否应该继续走nextcloud道路，因为浪费了太多时间了
- 决定明天继续走全部自己来的道路，最主要的工作其实只是要将db独立出来，另外要支持事务
- 看到PingCAP的新一期培训计划，线上部分原来是要发邮件提交作业，网页也没写，看zhihu的文章才知道

### 2019-07-08

- 今天放弃nextcloud，开始继续Java+MySQL搭建API
- 晚上导出了微信小程序的DB，清理了loginUser和user表，还没有导入新数据库

### 2019-07-09

- 将user表建立了，photo表还在进行中，完成了一边
- 下午去相城区搬了行李
- 搬家费用比我想象的便宜的多，几乎和打车费用一样，搬运费也很低
- 搬家的司机师傅是霍邱人
- 照司机师傅估计，那边的民房大概是三四百一个月
- 晚上选、买各种东西

### 2019-07-10

- 一早继续买买买
- 一天下了很多单
- 把旧的床单被套寄回家了，让妈帮忙处理
- 新买的三件套到了，晾干肯定来不及，花4块钱烘干20分钟，居然可以干，只有枕套有些地方没干，但晚上不用

### 2019-07-11

- 数据库迁移基本完成
- 收拾屋子

### 2019-07-12

- 解决首页先弹出登录页的问题
- 重写index页面的很多逻辑，比之前简化了非常多

### 2019-07-13

- 把一个请求包装成Promise
- Promise的资料

### 2019-07-14

- sshd_config GatewayPorts
- 如何用Java返回没有对应class的JSON结果: Map<String, Object>

### 2019-07-15

- Java JPA分页

### 2019-07-16

- 买了4本书：《枪炮、病菌与钢铁》、《议事规则》、《可操作的民主》、《金字塔原理》
- 上面的书都是李自然上次推荐的

### 2019-07-17

- wx.stopPullDownRefresh可以结束刷新动画
- app.json中的backgroundTextStyle、navigationBarTextStyle用来调整动画可见
- JPA中无法使用的sql注解无法使用isnull()

### 2019-07-18

- coscmd
- 40块钱买的联想键盘到了，感觉不错
- mac可以绑定键盘按键，可以绑定ctrl为command，但是fn没法绑定ctrl，然后ctrl在iterm2上没法输入。。
- wx.reLaunch可以解决wx.switchTab无法传参数的问题

### 2019-07-24

- WireGuard
- StreisandEffect/streisand GitHub (VPN)
- trailofbits/algo GitHub (VPN)
- Vagrant Multi Machine
- 买了Dell U2518D显示器，机器1380，运费63，显示器一个USB坏了
- MySQL Replication
- iptables
- 补了前面一周的笔记10+篇。。

[1]: https://learnku.com/articles/18418
[2]: https://blog.janguly.com/poster
[3]: https://developers.weixin.qq.com/community/develop/article/doc/00022cd6eb09a8933bb8b8f3d56013
[4]: https://www.ccarea.cn/archives/445
[5]: https://kodyk.myportfolio.com/content-learn-to-hack
