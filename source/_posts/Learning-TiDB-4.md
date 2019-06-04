---
title: TiDB学习笔记4
date: 2019-06-04 06:43:00
tags: [TiDB, TiKV]
---

打算在Mac上编译TiKV，按照TiKV的文档，还缺`cmake`，但安装时得到：
```
Error: cmake: "cxx11" is not a recognized standard
```
查了下，也没有什么解决方案，最后找到[这个答案][1]，安装binary，在设置`PATH`

在Mac上跑了很久，也没见结束，先停了，打算换到服务器上跑

在GCP上新申请了一个账号，可以跑一个8核52G内存的VM，安装了CentOS 7.6

#### 安装依赖
```shell
sudo yum install -y git cmake go clang # awk 和 make 已经有了
curl https://sh.rustup.rs -sSf | sh # 不能加sudo，加了之后装到/root用户目录下了（.cargo）
```

开始直接yum install了rust，之后有问题，只能删除
```
sudo yum -y remove rust 
```

### 按照官网操作步骤



```shell
git clone https://github.com/tikv/tikv.git
cd tikv
# Future instructions assume you are in this repository

rustup component add rustfmt-preview
make build
```

make build 失败
```
CMake Error at CMakeLists.txt:1 (cmake_minimum_required):
  CMake 3.1 or higher is required.  You are running version 2.8.12.2
```

切换到cmake3
```
sudo yum -y remove cmake 
sudo yum -y install cmake3 
mkdir ~/bin
ln -s /usr/bin/cmake3 ~/bin/cmake
```

make build继续失败
```
running: "cmake" "/home/ykg/.cargo/registry/src/github.com-1ecc6299db9ec823/grpcio-sys-0.4.2/grpc" "-DgRPC_INSTALL=false" "-DgRPC_BUILD_CSHARP_EXT=false" "-DgRPC_BUILD_CODEGEN=false" "-DCMAKE_INSTALL_PREFIX=/home/ykg/tikv/tikv/target/debug/build/grpcio-sys-b03572028937c5a0/out" "-DCMAKE_C_FLAGS= -ffunction-sections -fdata-sections -fPIC -m64" "-DCMAKE_C_COMPILER=/usr/bin/cc" "-DCMAKE_CXX_FLAGS= -ffunction-sections -fdata-sections -fPIC -m64" "-DCMAKE_CXX_COMPILER=c++" "-DCMAKE_BUILD_TYPE=Debug"
-- The CXX compiler identification is unknown
-- Configuring incomplete, errors occurred!
See also "/home/ykg/tikv/tikv/target/debug/build/grpcio-sys-b03572028937c5a0/out/build/CMakeFiles/CMakeOutput.log".
See also "/home/ykg/tikv/tikv/target/debug/build/grpcio-sys-b03572028937c5a0/out/build/CMakeFiles/CMakeError.log".

--- stderr
CMake Error at CMakeLists.txt:31 (project):
  The CMAKE_CXX_COMPILER:

    c++

  is not a full path and was not found in the PATH.

  Tell CMake where to find the compiler by setting either the environment
  variable "CXX" or the CMake cache entry CMAKE_CXX_COMPILER to the full path
  to the compiler, or to the compiler name if it is in the PATH.
```

没有`c++`，尝试
```
CXX=/usr/bin/clang++ make build
```
得到
```
CMake Error at /usr/share/cmake3/Modules/CMakeTestCXXCompiler.cmake:45 (message):
  The C++ compiler

    "/usr/bin/clang++"

  is not able to compile a simple test program.
```

最后不得不安装g++
```
yum install gcc-c++ libstdc++-devel
which c++
# /usr/bin/c++
make build
```

新错误
```
Could NOT find zlib (missing: ZLIB_LIBRARIES)
```

```
sudo yum -y install zlib-devel
make build
```

14:33 编译完成

```
cargo install cargo-watch
cargo watch -s "cargo check" # 这一句似乎并不会返回，似乎是一个监控的东西，只好另开窗口
```

新开窗口
```
make dev
```
结果得到`raftstore::store::util::tests::test_lease`测试失败，看到[这个PR][2]还在Open状态，说是时钟相关，在VMs中有问题

但执行单个test没事，全部通过
```
cargo test raftstore::store::util::tests::test_lease
```

继续
```
make release
```

得到
```
bash scripts/check-sse4_2.sh
checking bins for sse4.2
checking binary ./target/release/tikv-server for sse4.2
_ZN7rocksdb6crc32c10ExtendImplIXadL_ZNS0_L10Fast_CRC32EPmPPKhEEEEjjPKcm
error: ./target/release/tikv-server does not enable sse4.2
checking binary ./target/release/tikv-importer for sse4.2
_ZN7rocksdb6crc32c10ExtendImplIXadL_ZNS0_L10Fast_CRC32EPmPPKhEEEEjjPKcm
error: ./target/release/tikv-importer does not enable sse4.2
some binaries do not enable sse4.2
fix this by building tikv with ROCKSDB_SYS_SSE=1
```

看到TiKV的github也有类似[Issue][3]，但没有用，自己读了script，猜测是没有gdb，检查果然
```
gdb -batch -ex "disass/r _ZN7rocksdb6crc32c10ExtendImplIXadL_ZNS0_L10Fast_CRC32EPmPPKhEEEEjjPKcm" ./target/release/tikv-server
-bash: gdb: command not found
```

安装gdb
```
sudo yum -y remove gdb
```

scripts/check-sse4_2.sh执行通过，着手创建[PR][4]，再次阅读相关Issue和PR，发现原来已经说了是`gdb`问题。花了点时间整理提交了PR，感觉精力耗尽。



[1]: https://stackoverflow.com/questions/32185079/installing-cmake-with-home-brew
[2]: https://github.com/tikv/tikv/issues/3044
[3]: https://github.com/tikv/tikv/issues/4330
[4]: https://github.com/tikv/tikv/pull/4832
