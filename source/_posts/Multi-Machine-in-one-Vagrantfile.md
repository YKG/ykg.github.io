---
title: 多台机器组成局域网的Vagrantfile
date: 2019-07-24 09:06:51
tags: [Vagrant]
---

一个局域网，两台Ubuntu 18.04的例子([参考][2])：

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.define "alpha" do |alpha|
    alpha.vm.box = "ubuntu/bionic64"
    alpha.vm.network "private_network", ip: "10.0.0.10"
    alpha.vm.hostname = "alpha"
  end

  config.vm.define "beta" do |beta|
    beta.vm.box = "ubuntu/bionic64"
    beta.vm.network "private_network", ip: "10.0.0.11"
    beta.vm.hostname = "beta"
  end
end
```



官网的例子：

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: "echo Hello"

  config.vm.define "web" do |web|
    web.vm.box = "apache"
  end

  config.vm.define "db" do |db|
    db.vm.box = "mysql"
  end
end
```

官网[文档][1]

[1]: https://www.vagrantup.com/docs/multi-machine/
[2]: https://stackoverflow.com/questions/24867252/allow-two-or-more-vagrant-vms-to-communicate-on-their-own-network
