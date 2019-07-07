---
title: VM not created. Moving on...
date: 2019-07-07 10:14:14
tags: [Vagrant]
---

在创建vagrant box的时候出现了

```bash
$ vagrant package --base nextcloud_base_apache_mariadb_php72
==> nextcloud_base_apache_mariadb_php72: VM not created. Moving on...
```

查了，原因最后是--base后面跟的不是目的镜像名，或者也不是当前目录，而应该是要镜像的虚拟机名字，获取方法可以通过

```bash
vboxmanage list vms
```

得到

```
"getstarted_default_1562393959338_41212" {71998c94-4f46-4945-a4a8-de16be7e23a4}
"php-dev_default_1562399950327_11952" {c22f5cf7-5f3c-4d75-b494-72601851d21d}
"nextcloud_default_1562412596752_38492" {2656e76a-00be-4c08-a3c3-59734106c2f9}
"nextcloud_base_apache_mariadb_php72_default_1562463639749_62591" {1f7a5bbc-8b0a-4e86-b578-87d5c0dec7ab}
```

然后使用

```bash
vagrant package --base nextcloud_base_apache_mariadb_php72_default_1562463639749_62591
```

结果

```
==> nextcloud_base_apache_mariadb_php72_default_1562463639749_62591: Attempting graceful shutdown of VM...
    nextcloud_base_apache_mariadb_php72_default_1562463639749_62591: Guest communication could not be established! This is usually because
    nextcloud_base_apache_mariadb_php72_default_1562463639749_62591: SSH is not running, the authentication information was changed,
    nextcloud_base_apache_mariadb_php72_default_1562463639749_62591: or some other networking issue. Vagrant will force halt, if
    nextcloud_base_apache_mariadb_php72_default_1562463639749_62591: capable.
==> nextcloud_base_apache_mariadb_php72_default_1562463639749_62591: Forcing shutdown of VM...
==> nextcloud_base_apache_mariadb_php72_default_1562463639749_62591: Clearing any previously set forwarded ports...
==> nextcloud_base_apache_mariadb_php72_default_1562463639749_62591: Exporting VM...
==> nextcloud_base_apache_mariadb_php72_default_1562463639749_62591: Compressing package to: /Users/ykg/vagrant/nextcloud_base_apache_mariadb_php72/package.box
```

看了可以加上`--output`参数指定输出文件名
