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
- nc
- tc命令可以进行流量控制，比如限速

### 2019-07-25

- tcp-ping node.js
- ZGC

### 2019-07-26

- 赛图数据清洗、数据迁移，弄了一天

### 2019-07-27

- 处理小程序价格修改，正要收尾，紧急任务做赛事成绩
- 晚上弄的差不多了，夜里和老板去独墅湖
- 因老板谈话，生育问题，晚上入睡困难

### 2019-07-28

- Ad Hoc赛事成绩，快速响应

### 2019-07-29

- 继续支持赛事成绩
- geoip, ip2region
- 微信和它上面的小程序等没法进行gps持续记录

### 2019-07-30

- 将赛图换过数据库之后的版本上线了
- 要做数据库主备，做了一半，明天完成
- 中科凤麟招聘
- MS招聘，Intune

### 2019-07-31

- 买了性能之巅
- 早上浏览了极客时间上面买的Linux性能分析的CPU部分
- 数据库复制做好了，虽然费了不少时间。还是用docker好，不浪费时间

### 2019-08-01

- 书到了，下午看了一些
- 美国手机卡一直无服务，问客服无响应
- 拉取130路的状态存入数据库做好了

### 2019-08-02

- 翻《性能之巅》
- 买了4号去固始的车票
- 构思了下怎么web上传赛图图片，接入微信登录太麻烦，想自己生成二维码自己做同步

### 2019-08-03

- 回固始火车票退订
- 做《Linux性能优化实战》上的实验

### 2019-08-04

- eBPF，原以为是只跟网络相关，原来现在已经进内核了，相当于内核虚拟机，用来做监控的
- Promethus是监控工具，GRAFANA用来画dashboard的
- 不安之书,佩索阿 梁文道《一千零一夜》
- 打算做一个google镜像，自己用，看到了wen.lu做的这个功能的一个nginx模块ngx_http_google_filter_module

### 2019-08-05

- 早上出发回家
- 12306开始支持购买同一车次不同区间拼接了

### 2019-08-06

- smem, bcc tools, memleak, cachetop
- bcc tutorial
- ZFS, OpenZFS, illumos, DTrace
- iostat, iotop

### 2019-08-07

- 修改文件修改时间、创建时间
- What Every Programmer Should Know About Memory
- 晚上写第二版赛图Web上传

### 2019-08-08

- 昨夜不知道为何express-fileupload的mv函数有问题，移动后的文件比原文件小，而且还是随机大小，弄了好久不知道为什么，换成multer，nodejs库质量还是差，考虑是不是要python了
- 清除Mac的dns缓存
- iframe包装其他页面
- input的type里面有number类型，这样可以规避一些输入内容合法性检查
- Node.js的Crypto进行hash

### 2019-08-09

- jshell
- java历史
- jenkins
- rust clap cli处理
- 昨天和今天几个月之前的pingcap的docs的PR给通过了

### 2019-08-10

- 给姥爷上坟
- 晚上胡总发来发票查验ui原型，随即开始做了两个静态页面

### 2019-08-11

- 为发票查验添加过滤效果

### 2019-08-12

- 回苏州

### 2019-08-13

- /tmp目录并不一定是存储在内存中的，也可能是磁盘设备
- 红米Note7 Pro收到，很流畅
- 约YMM吃晚饭，没约成

### 2019-08-14

- 给6个之前招聘远程岗位做程序贡献分析的公司准备简历

### 2019-08-15

- 准备了给微软的英文简历，同时投递了stackoverflow上o'relly的remote岗位
- 家里的网出了问题，维修师傅帮忙弄好了一部分，之后都是我远程来操作。因为那边teamviewer版本安装的不对，导致每5分钟就要断掉，而且还要再等待2分钟，很烦。之前还不知道问题出在哪里，后来才知道，但又不敢升级，怕断了之后又没发操作可能更麻烦。后来找了ngrok，但因为在普通账户里，没有开放远程桌面，没成功，后来将就着弄好了。从中午一直弄到下午5点。

### 2019-08-16

- iView
- 看electron的东西
- 晚上刷抖音到0:20

### 2019-08-17

- electron
- 晚上和导演老板一起在关于吃吃饭，感觉不好吃，分量也少了一半。导演家看电影

### 2019-08-18

- 看了马前卒的所有《睡前消息》
- 继续pingcap的事情，做了project1的前两个part

### 2019-08-19

- 继续pingcap的project, 1完成，2开始
- 晚上导演到访，谈及要早做打算

### 2019-08-20

- 看了一天固始县的房价

### 2019-08-21

- 中午达哥转让一个留学生作业给我，自己出差没时间弄，忙到晚上10点，眼睛很难受

### 2019-08-22

- 早上一早5点起来继续做作业，8点40交出去一份
- 理了发，让老板帮忙内推了微软
- 下午给Morten发了求职邮件，未见回复
- 下午给信阳航空学院打了电话，问是否还在招人，让先投简历到邮箱，正要投递，微软来电，和recuriter聊了一下情况，交谈很融洽
- 晚上公交上金谷路站上来一姑娘，后来是住在同一公寓的，后悔没要联系方式，看明天是不是还能遇到

### 2019-08-23

- Morten未回复

### 2019-08-24

- 看码市的项目（看到有人要抖音的注册逆向，等有时间了看看搞一下）

### 2019-08-25

- 下午在导演家，看了两部电影，一部讲江西留守儿童的，一部讲印度的一场恐怖袭击
- 晚上说巴中那边有个做树莓派的活儿，比较急，准备过去，但时间可能要耽误微软面试，只能尽快

### 2019-08-26

- 白天全部用来看树莓派相关的东西了，最后还是没买
- 未见巴中那边的继续联系
- 下午取了沙拉等，晚上吃了一顿，还可以

### 2019-08-27

- 眉山论剑，下午回去后一直youtube到睡觉

### 2019-08-29

- nginx WebSocket配置
- 信用卡分期利率计算
- 将成绩部分小程序化
- 晚上刷抖音和快手等到1点钟

### 2019-08-30

- 把实时成绩移植到小程序上，但一直没有过审核
- 晚上把发票查验的东西又完善了下，晚上9点15分就睡觉了

### 2019-08-31

- 妈说要把密码锁，看了下网上ijj的锁，799，安装费100
- 上午把两月回顾写完了

### 2019-09-01

- 处理实时成绩的一点问题

### 2019-09-02

- 和XL那边对接好了发票查验接口

### 2019-09-03

- 中午接到微软通知面试，新加坡打来的，先用中文后转英语，英语都生疏了
- 9.6号上午11点面试
- 为面试焦虑，根部不想刷面试题

### 2019-09-04

- 早上看了一点儿《剑指offer》面试题
- 下午回来后制作了发票的样式图
- css文字分散对齐
- electron设置不允许调整窗口大小
- 没吃晚饭

### 2019-09-05

- 下午回来后一直刷抖音到晚上睡觉
- 晚上胡来电称又一个发票直连的单子，大约一百万，下周大概要准备下

### 2019-09-06

- 上午参加微软面试
- 面试之后即开始加紧开发发票查验的软件

### 2019-09-07

- 发票查验开发

### 2019-09-08

- 发票查验开发

### 2019-09-09

- 发票查验开发，晚上22:20输出了一个Mac版Demo，服务器部署到了bj上，启用了加密和ACL

### 2019-09-10

- 发现微软的职位被Archive了，据称是第一轮就挂了，很不愉快，晚上入睡困难，手机一直在放赵雷的《我们的时光》，大概凌晨3点多醒了一次听到还在放，给关了
- 一上午在弄GoAccess分析nginx访问日志，先用docker，有问题，后用apt安装也出现相同错误，后来解决了。
- 不知道有没有攻击扫描方的方式
- 下午调研关于服务器监控的工具，找到了nginx自己的有个服务监控，但是没有日志分析工具。知道了AppDynamics、DataDog等（在nginx官网的参考链接里）
- 之前在TS cph的时候知道了DataDog，我也使用了一个，收费，百万记录每月$2左右，但不知道少于这个量是不是按照1来算
- DataDog有个问题没有解决，就是access.log的访问权限没有，目前是手动chmod o+r所有访问日志，但对于新的日志必须得重新执行这个，官方介绍了其他方法，但没有试，似乎没有适合nginx的

### 2019-09-11

- 将发票查验软件更完善了下，主要是ui上的美化(datepicker/datetimepicker/dropdownlist，用的element.io的ui component)
- 尝试使用localForage进而使用IndexedDB，在网上搜的结果是配额是磁盘剩余容量的1/2，不过使用navigator.storage.estimate的结果却给出全部磁盘剩余容量
- 胡老板10点多来电称稳了，报价300w，分6/2/2三年付清，对方还价200w，说等中午吃饭时再谈，说也可以考虑5/3/2，问我的意见，我说都可以。晚上来电称还是300w，说第一笔是150w，估计是5/3/2方案，对方还要回去考虑，预计下周2之前有结论
- 早上问ms recruiter有什么feedback，说是到公司发给我，然后到现在也没有下文
- 瀚阑的钱到了

### 2019-09-12

- localForage的api没法进行过滤，只有iterate，放弃了，转而使用dexie
- 夜里完成了dexie替换localStorage，明天处理filter aside的处理
- 晚上和老板、导演一起吃了顿饭，然后去独墅湖逛了逛

### 2019-09-13

- filter处理完成
- 去老板家一起吃了顿饭
- 生成了基础版xlsx报表，表头和样式还需要进一步处理
- 和老板一起到附近的苏州农村地区转了转

### 2019-09-14

- 折腾了半天，结果js-xlsx不能加样式，只有Pro收费版提供，放弃，转而使用exceljs
- 加入了导出excel的进度条（只在任务栏logo上加，不在主界面）
- 16点已经累瘫，块19点起来，头疼
- autosub

### 2019-09-15

- 昨晚睡不着，刷抖音，刷到凌晨5点睡，10点起
- 把近期看的一些东西放到blog了
- 查了下软件保护方面的维基百科
- 看了《他乡的童年》前3集

### 2019-09-16

- 把发票查验的配置页面ui逻辑晚上了下，可以存储配置信息了
- iData每天可以下载5篇论文
- 发现被ddos了超过24小时，170+G流量，停机，换了ip

### 2019-09-17

- 深圳方面无消息
- azure的服务器都不能访问ss服务了，只有tw尚可，wireguard还可以用
- 读了byvoid的《Google工作这四年》

### 2019-09-18

- 发票查验做了一些修修补补
- 看加密通信相关的东西，aes原来是对称加密，想借鉴wireguard的加密方法，看它的白皮书里说是拿Curve25519做认证的，数据加密（看重速度）用的ChaCha20Poly1305
- Curve25519查了下算是当前最高等级的加密方法了
- 看wg的白皮书

### 2019-09-19

- 看密码学相关的东西
- 看《爱，死亡和机器人》

### 2019-09-20

- 去独墅湖图书馆借了3本书，2本密码学相关，1本2019前沿科技动态
- 看《爱，死亡和机器人》
- 试用了下webpack
- 使用百度脑图整理知识库

### 2019-09-21

- 刷抖音知道了伞兵15勇士先遣队，进而一直在刷汶川相关的东西

### 2019-09-22

- Prometheus + Grafana + Node Exporter和两个可以使用node exporter数据的dashboard配置好了
- 装了了Prometheus lua nginx的exporter，但没有配合的dashboard，费了好大劲，差不多白弄了
- 看了《武状元苏乞儿》

### 2019-09-23

- 调研日志分析工具，发现没有适合单机的工具，还是继续使用datadog吧
- 因为nginx的nginx_status也被日志收录进去了（nginx amplify也要使用，datadog agent似乎也要用，每隔20s一次访问），使用access_log off指令关闭了这个url的访问日志
- 为了对抗网站扫描，希望访问那些php的和刷.git目录的能够保持静默，什么也不返回。使用return 444服务端主动关闭，但觉得还不够，想发RST，后来查到SO_LINGER参数，进而了解到reset_timedout_connection指令，但指令在1.15.2版本才加入，服务器上的版本还是1.14，升级了nginx，然后加了这个指令，符合预期，这是wireshark抓来，貌似没有RST报文，服务器没有再发响应报文
- 定制datadog的日志格式，grok
- 看《走近科学》到深夜
- 下午从2点睡到5点

### 2019-09-24

- logrotate
- nginx USR1
- 在datadog上把日志弄好了。logrotate的postrotate脚本使用setfacl给access.log加上dd-agent访问权限
- 关闭所有停止的azure服务器，把sgp机器升级的8g内存，为了使用elk，监控nginx日志
- 装了太多agent，把端口都封了，停了agent，都使用elk，用的elk docker，没弄通bj到sgp的log
- 看了taobao各种虚拟机


### 2019-09-25

- 发现qcloud现在有企业新用户的优惠，2c8g5m的机器1790，想买
- 知道了aws lightsnail，aws淘宝可以买$150代金券（实际是学生优惠，网上有神奇方法）
- 胡老板发来截图，告知对方还价210w，我觉得不好，说还是走服务模式，闭源，他说不要。和导演说，我说还要和销售分，导演说销售分30%，如果谈下来建议新开个公司弄
- 停了elk docker，按照官方的方法做，发现原来logstash是非必须的，简单的情况beats就可以完成功能了，估计es也逐渐在让logstash变成optional的
- ps lstart
- namei -m
- sudo timedatectl set-timezone Asia/Shanghai
- elasticsearch -d deamon模式
- 不知道怎么给nginx log的dashboard加展示字段，es的文档还是欠缺和简陋
- nohup &, SIGHUP, screen/tmux，后台运行程序的方法（因为之前java跑的es和kibana都停了，虽然用的&，基础东西都熟啊）

### 2019-09-26

- 为了知道怎么结合使用ingest和filebeat，找了很多资料，但都不好。[这一篇文章][6]不错，最后终于把nginx access log的$host字段提取出来了，后来使用kibana的dashboard api增加了access log的展示列
- filebeat和ingest、es的大致关系有点了解了
- 瀚阑那边说下月中要弄图片直播，估计时间很紧（即使只有这个）
- 下午任总来电了解情况，了解开票的事情（查验得抓紧出一个完成版了，开票不知有没有时间弄）
- 右手胳膊都痉挛了，天天时间都在用电脑键盘
- 取了《代码虚拟与自动化分析》

### 2019-09-27

- 一整天在给nginx加入流控和防御请求类似`*.php`的请求，用fail2ban给封了
- 和胡了解下谈判进展，后来问及怎么分钱的事情，推说以后再谈，我要求先说明，告知平分方案，我提议30-70方案，他还让我考虑，双方都不考虑对方方案。往后我也不打算接受调和方案，甚至过了今天打算只接受15-85方案。
- 浏览了下这个文章[Building a Security Shield for Your Applications with NGINX & Wallarm][7]
- 晚上把system log和几个metrics都上了

### 2019-09-28

- start-stop-daemon
- nginx的directive是次序敏感的

### 2019-09-29

- ssllabs给了网站A+评分
- Emacs退出命令Ctrl+X Ctrl+C
- 限制metricbeat上传数据量（数据量太大了，只上传固定几个process的metrics）[参考][8]

### 2019-09-30

- 这两天没在工作的时间还是为分成的事情烦扰，昨天胡发来一长段消息，最后还有敲诈性质，不打算回复。倒是感受了下别人给自己发长微信，结论：永远不给任何人发长微信。
- iptables drop/reject icmp not reachable [link][http://www.chiark.greenend.org.uk/~peterb/network/drop-vs-reject]
- pwnat
- nps
- hping3/nping(nmap)
- 菌草
- pystun [https://doc-kurento.readthedocs.io/en/6.9.0/knowledge/nat.html#types-of-nat-in-the-real-world]
- 使用hping3直接发包做了会儿实验

### 2019-10-01

- 做赛图的海报分享
- scade调研

### 2019-10-02

- scade
- dsvpn

### 2019-10-03

- dsvpn
- 弄赛图前端
- 赛图上传迁移到bj服务器

### 2019-10-04

- 美化相册列表完成
- 微信审核不通过，要求提交icp备案材料和进行敏感内容检查
- 夜里发现图库第一张图是习大大的照片，对比时间，上传时间16:52，不通过通知是16:54下发的，猜测是微信的审核人员测试上传的

### 2019-10-05

- 加入了上传图片敏感内容检查
- 批量上传web入口加入敏感内容检查
- 赛图评论部分加入敏感内容检查

### 2019-10-06

- 试用jsproxy（Cloudflare Workers）成功，但看项目主页，因为网址是固定的，说有Cookie风险，建议在隐身模式浏览。网页速度飞快，youtube尚可，但不及dsvpn
- 赛图再次审核不通过，原因iOS禁止虚拟物品售卖
- 夜里以为明白了wg的配置，想把完全vpn实现了，结果证明想法没有被验证。但顺势尝试了下iptables，调试几条后，通了，很高兴

### 2019-10-07

- conntrack，这个是nat的关键
- pkg-config

### 2019-10-08

- 看房子4套，最后还是决定续住，签了3个月

### 2019-10-09

- 今天在西交大自习室自习，被西交大的人问及是哪个班的，后被识破，后来跟我说以后如果要用到8楼和他们申请下
- 晚上联系了童童，了解一下验票的成本
- 和YR哥联系了，建议开个公司，注册下专利，如果不行，他可以帮忙联系

### 2019-10-10

- 开发赛图的海报功能


[1]: https://learnku.com/articles/18418
[2]: https://blog.janguly.com/poster
[3]: https://developers.weixin.qq.com/community/develop/article/doc/00022cd6eb09a8933bb8b8f3d56013
[4]: https://www.ccarea.cn/archives/445
[5]: https://kodyk.myportfolio.com/content-learn-to-hack
[6]: https://gryzli.info/2019/04/21/working-with-ingest-pipelines-in-elasticsearch-and-filebeat/
[7]: https://www.nginx.com/blog/build-application-security-shield-with-nginx-wallarm/
[8]: https://discuss.elastic.co/t/storage-estimate-for-metricbeat/81878

