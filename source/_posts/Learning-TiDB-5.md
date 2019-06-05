---
title: TiDB学习笔记5
date: 2019-06-05 08:34:28
tags: [TiDB, TiKV]
---

为了了解需要的开发机器配置，分别在Azure上和GCP上测一下速度。

Azure：2c4G
GCP: 8c52G

但后来感觉很耗时间，只在GCP上做了编译，新建了一个16c32g的机器。

在gcp上，ubuntu-16c-32g
```shell
./check-requirement
sudo apt install cmake
./build
```

结果是
```
/usr/local/go/src/runtime/map.go:65:2: bucketCntBits redeclared in this block
	previous declaration at /usr/local/go/src/runtime/hashmap.go:64:18
```

说是因为旧版本go存在导致的,删除`/usr/local/go`重新安装go

```shell
rm -rf /usr/local/go
./check-requirement
./build
```

然后又得到
```
Could NOT find zlib (missing: ZLIB_LIBRARIES)
```

在ubuntu上安装zlib库
```
sudo apt install zlib1g-dev
```

```shell
sudo apt install gdb
./build
```

```
Finished release [optimized + debuginfo] target(s) in 21m 39s
```

开始：Wed Jun  5 07:06:28 UTC 2019
结束：Wed Jun  5 07:32:18 UTC 2019
耗时约26min

执行`check-sse4_2.sh`的时候，还是出了问题：
```
bash scripts/check-sse4_2.sh
checking bins for sse4.2
checking binary ./target/release/tikv-server for sse4.2
_ZN7rocksdb6crc32c10ExtendImplIXadL_ZNS0_L10Fast_CRC32EPmPPKhEEEEjjPKcm
gdb -batch -ex "disass/r _ZN7rocksdb6crc32c10ExtendImplIXadL_ZNS0_L10Fast_CRC32EPmPPKhEEEEjjPKcm" ./target/release/tikv-server 2> /dev/null | grep ">:.*f2.*0f 38.*crc32"
error: ./target/release/tikv-server does not enable sse4.2
checking binary ./target/release/tikv-importer for sse4.2
_ZN7rocksdb6crc32c10ExtendImplIXadL_ZNS0_L10Fast_CRC32EPmPPKhEEEEjjPKcm
gdb -batch -ex "disass/r _ZN7rocksdb6crc32c10ExtendImplIXadL_ZNS0_L10Fast_CRC32EPmPPKhEEEEjjPKcm" ./target/release/tikv-importer 2> /dev/null | grep ">:.*f2.*0f 38.*crc32"
error: ./target/release/tikv-importer does not enable sse4.2
some binaries do not enable sse4.2
fix this by building tikv with ROCKSDB_SYS_SSE=1
Makefile:80: recipe for target 'release' failed
make: *** [release] Error 1
```

单独执行gdb那条:
```shell
gdb -batch -ex "disass/r _ZN7rocksdb6crc32c10ExtendImplIXadL_ZNS0_L10Fast_CRC32EPmPPKhEEEEjjPKcm" ./target/release/tikv-server
```

得到：
```
warning: Missing auto-load script at offset 0 in section .debug_gdb_scripts
of file /home/ykg/repo/docs/scripts/deps/tikv/target/release/tikv-server.
Use `info auto-load python-scripts [REGEXP]' to list them.
No symbol '_ZN7rocksdb6crc32c10ExtendImplIXadL_ZNS0_L10Fast_CRC32EPmPPKhEEEEjjPKcm' in current context
```

在CentOS 8c52g的机器上测试这条没问题

## 检查tikv编译时间

TiKV的Makefile中
```
release:
	cargo build --no-default-features --release --features "${ENABLE_FEATURES}"
```
改为
```
release:
	env RUSTFLAGS=-Ztime-passes cargo build --no-default-features --release --features "${ENABLE_FEATURES}"
```
可以打印详细时间

发现`kvproto`和`tikv`两个似乎编译时间非常久，明天再检查下

在8c52g centos7.6机器上，执行`make`:
```
[ykg@instance-1 tikv]$ date;make;date
Wed Jun  5 15:21:09 UTC 2019
cargo build --no-default-features --release --features " jemalloc portable sse no-fail"
  Downloaded sys-info v0.5.7
   Compiling sys-info v0.5.7
   Compiling tikv_util v0.1.0 (/home/ykg/tikv/tikv/components/tikv_util)
   Compiling engine v0.0.1 (/home/ykg/tikv/tikv/components/engine)
   Compiling tikv v3.0.0-beta.1 (/home/ykg/tikv/tikv)
    Finished release [optimized + debuginfo] target(s) in 15m 28s
bash scripts/check-sse4_2.sh
skipping sse4.2 check - gdb is required
Wed Jun  5 15:36:43 UTC 2019
```

在ubuntu-16c-32g上，执行`make clean`, `date;make;date`，得到
```
Wed Jun  5 15:52:55 UTC 2019
cargo build --no-default-features --release --features " jemalloc portable sse no-fail"
   Compiling libc v0.2.45
   ...
   Compiling kvproto v0.0.1 (https://github.com/pingcap/kvproto.git?branch=release-3.0#11e0e6d3)
   Compiling cop_datatype v0.0.1 (/home/ykg/repo/docs/scripts/deps/tikv/components/cop_datatype)
   Compiling tipb_helper v0.0.1 (/home/ykg/repo/docs/scripts/deps/tikv/components/tipb_helper)
   Compiling rocksdb v0.3.0 (https://github.com/pingcap/rust-rocksdb.git#5adf5b84)
   Compiling engine v0.0.1 (/home/ykg/repo/docs/scripts/deps/tikv/components/engine)
   Compiling log_wrappers v0.0.1 (/home/ykg/repo/docs/scripts/deps/tikv/components/log_wrappers)
   Compiling tikv v3.0.0-beta.1 (/home/ykg/repo/docs/scripts/deps/tikv)
    Finished release [optimized + debuginfo] target(s) in 22m 13s
bash scripts/check-sse4_2.sh
skipping sse4.2 check - gdb is required
Wed Jun  5 16:15:14 UTC 2019
```

再次执行make，几乎不需要时间。

修改一个字符串，重新
```shell
date;make;date
```
得到
```
Wed Jun  5 16:22:29 UTC 2019
cargo build --no-default-features --release --features " jemalloc portable sse no-fail"
   Compiling tikv v3.0.0-beta.1 (/home/ykg/repo/docs/scripts/deps/tikv)
    Finished release [optimized + debuginfo] target(s) in 15m 39s
bash scripts/check-sse4_2.sh
skipping sse4.2 check - gdb is required
Wed Jun  5 16:38:14 UTC 2019
```

## 其他

发现很多文档没有及时更新，比如Go的版本现在已经要求`1.12`了，但`check-requirement.sh`里面还是`1.10`，另外`gdb`、`zlib`也是要安装的，但check脚本里没有。

