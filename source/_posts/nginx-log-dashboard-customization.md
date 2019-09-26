---
title: 定制Kibana的nginx日志
date: 2019-09-26 20:30:33
tags: [Kibana, ELK, nginx, log, Dashboard]
---

猜测的原理：

- filebeat根据enabled的module，在执行setup指令时将对应module的default pipeline注册到es上
- filebeat把access.log的一行行原始数据发给es
- es拿到数据后，使用ingest的pipeline转换成提取字段后的json
- es将转化完的json进行index
- 可检索

所以猜测所有数据处理都是放在es上弄的，filebeat只是发射器，它不参与转换，虽然pipeline的初始化是在filebeat这边来弄的，但作用只是根据激活的module，将对应的pipeline信息构造成`POST _ingest/pipeline/xxx`的参数，注册到es里。

此外es索引更新似乎有延迟，使用`filebeat setup --pipeline modules nginx`更新defaut-pipeline.json，虽然restful看是更新了，但数据要过一段时间才重新整理好。

### 注意到两个情况

- 延迟
    * filebeat setup --pipeline更新pipeline
    * 发现x_forward_for字段忘了加remove指令了
    * filebeat setup --pipeline更新，加上remove
    * 重发请求，观察获取的数据，结果还是有x_forward_for
    * 过一段时间发现，x_forward_for没了（似乎还是同一请求，不确信）

- 旧数据不应用pipeline更新
    * 因为加入了nginx.request.host字段，看过去24小时的记录列表，发现之前的这个字段都是`-`，最新的都是有host字段值的（按理，似乎与上面的情况冲突了）
    
### 配置dashboard过程

- 思路
    * 首先把host字段提取出来
    * 知道怎么配置dashboard，把对应的字段展示出来

- 提取字段
    * 使用ingest的pipelie中的Grok进行匹配（和datadog一样，datadog已经弄成功过了，所以很有把握）
        * 关键是知道怎么操作（简单的方法：修改`/usr/share/filebeat/module/nginx/access/ingest/default.json`，然后`filebeat setup --pipeline modules nginx`告诉es更新pipeline，可能还要重启filebeat一下）
    * 访问服务器，确认在dashboard那边看新的访问日志里面有新加入的字段

- 字段展示
    * Kibana上面没有可以配置dashboard字段的方法，只能该panel的名字
    * 了解到有Dashboard的API提供（Export/Import）
    * 把Dashboard在Kibana上Clone了一份，然后根据类似uuid的id，进行export
    * 对export的json，搜索url.original知道了对应的panel配置，给`nginx.request.host`加到了前面
    * import时修改了对应的panel的id，原来是`xxxx-ecs`，改成了`xxxx-ngx`，新的dashaboard的id也改成`11223344-`前缀，改了title，执行import。告知其他panel都是409，检查dashboard列表，已经有新的dashboard了，但没有对应列
    * 再检索url.original，发现原来dashboard上对应的`panelsJSON`也有一份字段列表，加上了`nginx.request.host`，再试，成功


### 致谢

只有这篇文章才是好的，其他都是垃圾：[Working With Ingest Pipelines In ElasticSearch And Filebeat][1]

[1]: https://gryzli.info/2019/04/21/working-with-ingest-pipelines-in-elasticsearch-and-filebeat/
