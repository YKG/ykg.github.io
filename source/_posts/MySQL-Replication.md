---
title: MySQL Replication
date: 2019-07-24 09:01:08
tags: [MySQL]
---

几天前做了一下MySQL复制的实验。这里记录一下操作日志。

### 从镜像下载vagrant ubuntu box

```
vagrant box add \
https://mirrors.tuna.tsinghua.edu.cn/ubuntu-cloud-images/bionic/current/bionic-server-cloudimg-amd64-vagrant.box \
--name ubuntu/bionic64
```

### 重命名Vagrant box

```
$ vagrant box list
ubuntu/bionic (virtualbox, 0)

$ ls ~/.vagrant.d/boxes
ubuntu-VAGRANTSLASH-bionic/

$ cd ~/.vagrant.d/boxes
$ mv ubuntu-VAGRANTSLASH-bionic/ ubuntu-VAGRANTSLASH-bionic64

$ vagrant box list
ubuntu/bionic64 (virtualbox, 0)
```

> 之所以有这么一出，是因为上面从清华的vagrant box源下载的被命名为不带64结尾的了

### SSH auth method: private key 卡住并超时

根据[这个][1]，停用了hyper-V，然后重启确实可以了。试了其他方式，但都没用。

### MySQL版本

```
mysql> show variables like 'version%';
+-------------------------+-----------------------------+
| Variable_name           | Value                       |
+-------------------------+-----------------------------+
| version                 | 5.7.26-0ubuntu0.18.04.1-log |
| version_comment         | (Ubuntu)                    |
| version_compile_machine | x86_64                      |
| version_compile_os      | Linux                       |
+-------------------------+-----------------------------+
4 rows in set (0.00 sec)

mysql> ^DBye
vagrant@master:~$ mysql -V
mysql  Ver 14.14 Distrib 5.7.26, for Linux (x86_64) using  EditLine wrapper
```

### MySQL复制

- 主从都配置下面的账号

```sql
GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* \
TO repl@'10.0.0.%' IDENTIFIED BY 'p4ssword';
```

- my.cnf

master：

```conf
log_bin = mysql-bin
server-id = 10
```

slave:

```conf
log_bin = mysql-bin
server-id = 11
relay_log = /var/lib/mysql/mysql-relay-bin
log_slave_updates = 1
read_only = 1
```

- service mysql reload

- on slave: change master

```sql
change master to master_host='10.0.0.10',
master_user='repl',
master_password='p4ssword',
master_log_file='mysql-bin.000001',
master_log_pos=0;
```

- on slave: start slave

- check status

* show slave status \G
* show processlist \G




[1]: https://github.com/hashicorp/vagrant/issues/8157#issuecomment-458549241


