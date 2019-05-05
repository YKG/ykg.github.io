---
title: report-public-IP-address
date: 2019-05-05 12:47:08
tags:
---

本文记录如何获取内网的外部公网IP，功能类似于DDNS。

### 原理

在公网部署一台服务器，用内网的机器定期连接该服务器，服务器记录连接的客户端公网IP，通过获取服务器记录下的IP即可得到内网的公网IP

### 背景

- 内网有一台QNAP的NAS
- 内网通过一级路由器外接入公网
- 路由器WAN IP为公网IP

### 目的

高速远程访问NAS资源

### 现状

使用myQNAPcloud访问NAS资源

### 问题

myQNAPcloud可以访问，但速度慢，访问的资源都是大文件

### 端口转发方案

- 在路由器上开启`端口转发`，将NAS的Web服务端口暴露到公网
- 访问者直接使用`http://<路由器公网IP>:<映射端口>`访问内网NAS

### 端口转发方案存在的问题

`路由器公网IP` 是会变化的

### 动态公网IP问题的解决

- 尝试花生壳DDNS，没成功，原因未知
- 尝试docker版frp，也没成功，原因未知
- 自行设计方案，见下文

### 服务端

```javascript
const http = require('http');
const lastIP = {ip: "", ts: ""};
function now() {
    // sv-SE 是为了日期格式为 2019-05-05，h24为24小时制
    return new Date().toLocaleString('sv-SE-u-hc-h24', { timeZone: 'Asia/Shanghai' });
}
const start = now();
const server = http.createServer((req, res) => {
    const url = req.url;
    if (url === '/set') {
        const ip = res.socket.remoteAddress;
        const port = res.socket.remotePort;
        lastIP.ip = ip;
        lastIP.ts = now();
        res.end(`Your IP address is ${ip} and your source port is ${port}.`);
    } else if (url === 'up') {
        res.end(`up since ${start}`);
    } else {
        res.end(`IP: ${lastIP.ip}, ts: ${lastIP.ts}`);
    }
}).listen(21110);
```

### 客户端
```bash
ssh nas
watch -n 30 http://<服务器IP>:21110/set & # 每30s连接一下服务器，让服务器记录对应的公网IP
```

### 获取内网的公网IP

访问`htttp://<服务器IP>:21110`
