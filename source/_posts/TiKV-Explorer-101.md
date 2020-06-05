---
title: TiKV探索101
date: 2020-06-05 18:18:51
tags: TiKV
---


之前使用tiup工具部署了下，使用dashboard的时候发现cluster info里的主机信息有的错的，有的是缺的，后来还发现诊断报告里也有类似的问题，于是着手准备追踪看看然后提交PR。后来发现正好有个捉虫比赛的活动，于是全都报告到那边了。追踪过程中把那些信息的获取路径跟了下，知道了采集每个服务的主机信息（硬件信息）的时候是怎么实现的。算是第一次深入到TiKV的代码里。

为了更全面地了解TiKV，打算再系统地了解下它的实现。

印象中文档里提到TiKV是需要有PD配合才能使用，PD是一个主控机构，来对TiKV进行调度。

在TiKV的repo/docs/overview.md中介绍了TiKV的RawKV方式，提供两个接口，一个是裸K-V存储，一个是带事务的K-V存储。简单试用了一个简单的裸K-V的样例代码，只是有一点出乎意料，对于不存在的Key，Get接口取回的是一个length为0的数组（[]u8?），而不是预期的返回一个代表不存在对应Key错误代码或异常。

直接从cmd目录的两个bin作为入口来阅读，感觉有没有特别目标，于是想将它作为一个黑盒，先看对外给出的参数接口。
分别看了tikv-server的和tikv-ctl的，tikv-server的比较简单，除去指定config文件的选项，其他都比较好理解。
tikv-ctl不想自己编译，用tiup ctl下载了所有ctl包，用tiup ctl tikv命令看了下参数，还是比较多的，大部分都是跟具体实现关联比较大，试用了`--encode`和`--decode`，encode之后的内容decode不回来，或者任意给参数，decode也没有能正确返回的，不了解是不是decode没有正确实现。`--to-escaped`和`--to-hex`倒是没问题。

用tiup ctl tikv -h并不能看到tikv-ctl的help信息，看到的是tiup命令的help信息。可以用子命令help；tikv ctl tikv help

后面还是直接进对应路径，直接试用tikv-ctl，试用了`tikv-ctl --host 172.17.0.7:20160 cluster`和store看了下他们的clusterId和storeId，用tiup cluster display看的集群服务列表，tikv一项的端口是20160/20180，20180好像是tikv们之间的通信接口，--host指定20180端口是没有用的，只有20160端口可以支持tikv-ctl的命令。

觉得还是没啥头绪，于是打算看SQL执行路径，特别是Query/Select的路径。看TiDB那边的这个部分时，发现基本上要看的这些函数全都有注释，挺好的，其他的没有，刚好我这关注的都有，当然也很好理解，它们本来就是DB正常工作时的关键路径。

调用栈：

tidb-server/main.go::main()
tidb-server/main.go::runServer()
server/server.go::Run()
server/server.go::onConn()
server/conn.go::Run()
server/conn.go::dispatch()
server/conn.go::handleQuery()
server/conn.go::handleStmt()
server/driver_tidb.go::ExecuteStmt()
session/session.go::ExecuteStmt()
session/session.go::runStmt()
executor/adaptor.go::Exec()

追到这个地方，看了里面的实现，好像没有跟TiKV连上，也没看到那个地方是rs结果集是期待的路径。
今天就先追到这里。
