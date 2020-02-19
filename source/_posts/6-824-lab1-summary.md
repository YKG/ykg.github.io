---
title: 6.824 Lab1小结
date: 2020-02-19 20:32:07
tags: [分布式, MapReduce]
---

昨天终于把Lab1写完，用测试脚本跑了一遍，第一把就全通过了。

今天记一下这个过程中值得记录的一些点。

#### go的同步问题

有这样一个场景，对于每个任务，我在初始化时给它们的状态都是0，然后如果有worker来要任务，我分配出去后，会把状态设置为1，代表处理中或者running的意思，如果worker处理完会上报处理完这个消息，master接到之后会给状态设置为2，代表已处理完成。为了防止worker不上报处理结果（可能worker宕机，可能网络问题），需要在master这边对每一个下发的任务设置一个定时器，约定10s内如果没有收到状态报告，就认为worker处理这个任务失败，那么就把状态由1改回0，也就是恢复到未分配模式，等到下一轮任务分发时，可以重新下发这个任务。


```go
go func() {
    if status == 1 {
        time.Sleep(10 * time.Second)
        status = 0         // #1
    }
}

go func() {
    if status == 1 {
        status = 2         // #2
        taskDoneCount++    // #3
    }
}
```

但这样会出现数据竞争问题，就是上面代码中的场景：在定时器到期时，同时收到了worker的状态报告，这时两个线程竞争将1改为0和2。

如果执行顺序是`#2`、`#1`、`#3`，那么就会导致这个任务状态被重置为未分配模式，可是任务完成计数却增加了1。

> 写到这里我在想如果不使用那个计数器，似乎这样的竞争也是允许的，应为不同的覆盖结果并不影响状态一致性，都是可以接受的结果

于是我在想需要有互斥锁，来原子性地进行比较和赋值上面两个goroutine。看了sync.Mutex的文档，说建议使用high level的同步设施，看了下channel。然后处理成这样

```go
var c chan

go func() {
    status <- c
    if status == 2 {
        taskDoneCont++
    }
}

go func() {
    time.Sleep(10 * time.Second)
    c <- 0
}

go func() {
    c <- 2
}
```

这样感受下来，channel可以把多个输入源归集起来，然后按照顺序执行


#### worker处理逻辑

最开始想worker只处理一个任务，多次执行worker来执行所有任务，假设有M个任务，就需要M个worker来消化。后来想想可能不对，如果一个worker，它应该可以处理完一个之后再去要一个来，知道所有任务都结束才算完。于是设计了下面这样一个结构。

```go
for {
    reply := fetchMapTask()
    if reply.Status == "done" {
        break
    }
    if reply.Status == "ok" {
        result := doMapTask(reply.MapTask, mapf)
        mapNotifyMaster(reply.MapTask, result)
    } else {
        time.Sleep(time.Second)
    }
}

for {
    // same structure for reduce task
}
```

#### 健壮性

本身的程序有一定健壮性了，专门测试了领了任务后把worker终止掉，master那么正确超时重置了超时任务。worker处理过中还是可以有很多失败点的，只有在最后完成才发布正确通知，可以兜底，中间部分是可以有更多提示的，比如磁盘不足或者其他故障，如果能检测到，就应该及时向master报告异常情况，master可以在超时前就将任务重置。

另外就是master也可能挂掉，当前的程序相当于重新启动了，但实际上应该可以及时保存状态，下次可以基于已完成的任务再做未完成的。这个部分就没有在Lab1里做了。
