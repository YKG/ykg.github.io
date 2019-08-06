---
title: Linux性能优化-实验报告18
date: 2019-08-06 09:41:25
tags: [Perf]
---

```
# install sysstat docker
sudo apt-get install -y sysstat docker.io

# Install bcc
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 4052245BD4284CDD
echo "deb https://repo.iovisor.org/apt/bionic bionic main" | sudo tee /etc/apt/sources.list.d/iovisor.list
sudo apt-get update
sudo apt-get install -y bcc-tools libbcc-examples linux-headers-$(uname -r)
```

```
docker run --name=app -itd feisky/app:mem-leak
docker logs app
```

memleak并没有监测出来什么异常

```
root@perf:/home/ykg# /usr/share/bcc/tools/memleak -a -p $(pidof app)
Attaching to pid 6757, Ctrl+C to quit.
[02:31:39] Top 10 stacks with outstanding allocations:
[02:31:44] Top 10 stacks with outstanding allocations:
[02:31:49] Top 10 stacks with outstanding allocations:
[02:31:54] Top 10 stacks with outstanding allocations:
[02:31:59] Top 10 stacks with outstanding allocations:
```


