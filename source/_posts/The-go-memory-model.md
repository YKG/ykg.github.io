---
title: The Go memory model
date: 2020-02-21 21:45:52
tags: [Go]
---

看了一遍要求读的材料[The Go Memory Model][1]


主要讲的东西是关于程序执行顺序的。重点在多个goroutine一起运行时的情况。

为了要进行同步，可以使用channel、sync和sync/atomic包中的方法

简单总结：
- 如果在一个goroutine内，对于同一个变量执行顺序按照先后次序来
- 如果在不同goroutine中，那么两边的执行顺序完全不确定，除非使用上面的同步方式


#### 同步

- 初始化
	- P import Q 的话，Q的init happens before P的init
	- main.main happens after 所有 init 函数完成
- Goroutine创建
	- go语句启动一个新的goroutine happens before 这个新goroutine开始执行
- goroutine析构
	- goroutine的退出并不保证会happens before任意事件
- Channel通信
	- channel是用于goroutines之间同步的主要方式
	- 一个channel上的send happens before 在它之上的receive
	- channel的关闭 happens before 在它之上的 receive（说channel关闭导致的返回0值是个啥意思？）
	- 对于一个无缓冲channel，receive happens before send完成
	- 对于一个容量为C的channel，第k个receive happens before第k+C个send
- Lock
	- 对于sync.Mutex/sync.RWMutex变量L，次数N < M，第N次调用L.Unlock happens before 第M次调用L.Lock返回。（换句话说，就是只有先Lock才有后Unlock，Unlock的不能是没有被Lock的）
	- For any call to l.RLock on a sync.RWMutex variable l, there is an n such that the l.RLock happens (returns) after call n to l.Unlock and the matching l.RUnlock happens before call n+1 to l.Lock.（说得都不知是啥）
- Once
	- Once.Do(f)中的f的调用 happens before 整个Once.Do语句返回（就是f()执行完之后Once才返回）
	- Once.Do保证里面的函数只会被执行一次，即使多次调用Once

#### 错误的同步

这一节的意思是说，从一个goroutine来看，另一个goroutine中的执行过程并不保证和书写的次序一致，它里面的赋值语句之类的可能受到重排序影响，不应该在没有显示同步的情况下，对另一个goroutine的有任何期待
		

[1]: https://golang.org/ref/mem
