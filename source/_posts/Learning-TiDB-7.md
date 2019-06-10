---
title: Learning TiDB 7
date: 2019-06-10 12:23:16
tags: [TiDB, TiKV]
---

今天尝试[Try Two Types of APIs][1]，可以直接访问TiKV

执行该命令时
```
go get -u github.com/pingcap/tidb@master
```

得到
```
go: google.golang.org/genproto@v0.0.0-20181004005441-af9cb2a35e7f: unrecognized import path "google.golang.org/genproto" (https fetch: Get https://google.golang.org/genproto?go-get=1: dial tcp 216.239.37.1:443: i/o timeout)
go: error loading module requirements
```

使用代理可以解决


试用rawkv时，得到
```
build rawkv-demo: cannot load github.com/pingcap/pd/pd-client: cannot find module providing package github.com/pingcap/pd/pd-client
```

解决：
```shell
GO111MODULE=on go get -u github.com/pingcap/tidb@master
```
其实文档开头提到了这个



执行示例代码时，得到
```
go: github.com/golang/lint@v0.0.0-20190409202823-959b441ac422: parsing go.mod: unexpected module path "golang.org/x/lint"
go: finding github.com/prometheus/common v0.4.0
go: finding golang.org/x/tools v0.0.0-20190425150028-36563e24a262
go: finding golang.org/x/sys v0.0.0-20190606165138-5da285871e9c
go: finding golang.org/x/sys v0.0.0-20181205085412-a5c9d58dba9a
go: finding google.golang.org/genproto v0.0.0-20190418145605-e7d98fc518a7
go: finding github.com/ugorji/go v1.1.5-pre
go: finding github.com/jstemmer/go-junit-report v0.0.0-20190106144839-af01ea7f8024
go: finding github.com/prometheus/tsdb v0.7.1
go: finding golang.org/x/net v0.0.0-20190603091049-60506f45cf65
go: finding github.com/coreos/bbolt v1.3.2
go: finding github.com/coreos/go-semver v0.3.0
go: finding golang.org/x/lint v0.0.0-20190301231843-5614ed5bae6f
go: finding golang.org/x/crypto v0.0.0-20181203042331-505ab145d0a9
go get: error loading module requirements
```

参考这个[链接][2]，解决：
```shell
go mod edit -replace github.com/golang/lint=golang.org/x/lint@latest
```

把那些前提配置到Dockerfile之后，启动docker还遇到
```
go get github.com/pingcap/tidb@master: git init --bare in /go/pkg/mod/cache/vcs/023ec28de881fe16123b69e600d28e8671b1fa6b70863a0d65ca08ea4bcc7d6d: exec: "git": executable file not found in $PATH
```

参考这个[链接][3]，解决：
```shell
FROM golang:alpine
RUN apk add git # 安装git
```


[1]: https://github.com/tikv/tikv/blob/master/docs/clients/go-client-api.md
[2]: https://github.com/golang/lint/issues/446#issuecomment-483638233
[3]: https://stackoverflow.com/questions/36044275/golang-go-get-command-showing-go-missing-git-command-error
