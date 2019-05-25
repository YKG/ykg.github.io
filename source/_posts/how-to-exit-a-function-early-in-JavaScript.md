---
title: 怎样在JavaScript中尽早返回
date: 2019-05-25 23:49:35
tags: [JavaScript]
---

为了健壮性，要对每一步操作进行执行结果检查，代码写出来很臃肿，不知道怎么精简。

例子：
```JavaScript
async function func1(){
    let res
    try {
        let res = await db.call()
    } catch(e) {
        console.error(e)
        return {
            err: true,
            msg: 'fail in func1'
        }
    }
    return {
        err: false,
        data: res
    }
}

// func2 ...
// func3 ...

async function main(){
    let res = await func1()
    if (res.err) {
        return {
            err: true,
            msg: 'fail in func1'
        }
    }

    res = await func2()
    if (res.err) {
        return {
            err: true,
            msg: 'fail in func2'
        }
    }

    res = await func3()
    if (res.err) {
        return {
            err: true,
            msg: 'fail in func3'
        }
    }

    return {
        err: false,
        msg: 'ok'
    }
}
```

因为看过UNP中作者使用Wrapper函数，所以希望也能将`func1`之类的被调用函数直接取代`main`来返回。期望的形态应该是：
```JavaScript
async function func1(){
    let res
    try {
        let res = await db.call()
    } catch(e) {
        console.error(e)
        EXIT({                      // <---- 希望能从这里接管 main 的控制流
            err: true,
            msg: 'fail in func1'
        })
    }
    return {
        err: false,
        data: res
    }
}

// func2 ...
// func3 ...

async function main(){
    let res = await func1()        // 如果上面的可以实现，这里就相当于自带错误处理，出错时可以提前返回
    res = await func2()
    res = await func3()
    
    return {
        err: false,
        msg: 'ok'
    }
}
```

问题是，在`func1`中`return`之类的都只能回到调用方，只有`throw`才可能印象到控制流，throw当然也行，类似
```JavaScript
async function main(){
    try {
        let res = await func1()
        res = await func2()
        res = await func3()
    } catch (e) {
        return {
            err: true,
            msg: e
        }
    }
    
    return {
        err: false,
        msg: 'ok'
    }
}
```
一般来说，会要求`try`的部分尽可能小，不知道对于JavaScript来说是不是也是这样的，把所有都包起来似乎不是很好的处理方式。


在StackOverflow上看到[这个问答][1]介绍了怎么给一个function包上catch。

然后发现[另一篇][2]，是express的next，貌似和我想要的很像，看了下express的中间件，似乎正式解决我的问题，只是它的这个是针对Web的，不知道对于非Web的有没有专门的对应东西。另外有时间还是要学习下express和koa。


[1]: https://stackoverflow.com/questions/41349331/is-there-a-way-to-wrap-an-await-async-try-catch-block-to-every-function/41350872#41350872
[2]: https://stackoverflow.com/questions/14709802/exit-after-res-send-in-express-js
