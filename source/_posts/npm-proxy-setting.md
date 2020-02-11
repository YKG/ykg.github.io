---
title: npm代理设置
date: 2020-02-11 20:33:37
tags: [proxy, npm]
---


```bash
npm config proxy socks5://127.0.0.1:1080
npm config https-proxy socks5://127.0.0.1:1080
# 上面两个也可以使用 npm config edit 进行记事本编辑
```

但上面设置好了之后，遇到了

```
npm ERR!       E418
npm ERR! 418 I'm a teapot:
```

看到[how-do-i-solve-the-npm-418-im-a-teapot-error-when-trying-to-use-npm-install][1]说要将`npmrc`的`registry`URL从`http`协议改成`https`

```
npm config set registry https://registry.npmjs.org/
```

改了之后确实有效

[1]: https://stackoverflow.com/questions/51524828/how-do-i-solve-the-npm-418-im-a-teapot-error-when-trying-to-use-npm-install

Error: Could not fork child process: There are no available terminals (-1).