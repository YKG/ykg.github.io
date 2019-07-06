---
title: Vagrant
date: 2019-07-06 20:14:23
tags: [Vagrant]
---

创建一个ubuntu18.04虚拟机的过程：

```bash
vagrant init ubuntu/bionic64 # 这一步会生成Vagrantfile
vagrant up

vagrant ssh # ssh进入vm，用户为vagrant
```

`/vagrant`目录默认共享宿主机器上启动vagrant命令的当前目录


可以通过`config.vm.provision`为vm预装软件

比如预装个apache2

- bootstrap.sh

```bash
#!/usr/bin/env bash

apt-get update
apt-get install -y apache2
if ! [ -L /var/www ]; then
  rm -rf /var/www
  ln -fs /vagrant /var/www # 这样就可以在host机器上修改文件，必须要进入vm了
fi
```

- Vagrantfile

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.provision :shell, path: "bootstrap.sh"
end
```

暴露端口（端口转发）


```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.provision :shell, path: "bootstrap.sh"
  config.vm.network :forwarded_port, guest: 80, host: 4567
end
```

关闭相关

```bash
vagrant suspend
vagrant halt
vagrant destory # 彻底销毁
```