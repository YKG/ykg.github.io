---
title: Linux性能优化-实验报告05
date: 2019-08-03 14:31:31
tags: [Perf]
---


```
docker run --name nginx -p 10000:80 -itd feisky/nginx
docker run --name phpfpm -itd --network container:nginx feisky/php-fpm
```

```bash
# 并发 10 个请求测试 Nginx 性能，总共测试 100 个请求
ab -c 10 -n 100 http://13.67.110.15:10000/
```

```
ubuntu@VM-0-5-ubuntu:~$ ab -c 10 -n 100  http://13.67.110.15:10000/
ab: invalid URL
Usage: ab [options] [http[s]://]hostname[:port]/path
```

最后必须要加上斜杠


```
ubuntu@VM-0-5-ubuntu:~$ ab -c 10 -n 100  http://13.67.110.15:10000/
This is ApacheBench, Version 2.3 <$Revision: 1807734 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 13.67.110.15 (be patient).....done


Server Software:        nginx/1.15.4
Server Hostname:        13.67.110.15
Server Port:            10000

Document Path:          /
Document Length:        9 bytes

Concurrency Level:      10
Time taken for tests:   11.712 seconds
Complete requests:      100
Failed requests:        0
Total transferred:      17200 bytes
HTML transferred:       900 bytes
Requests per second:    8.54 [#/sec] (mean)
Time per request:       1171.152 [ms] (mean)
Time per request:       117.115 [ms] (mean, across all concurrent requests)
Transfer rate:          1.43 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:      101  527 761.8    103    3138
Processing:   194  500 428.9    345    2866
Waiting:      194  371 238.7    244    1348
Total:        296 1027 837.8    768    3976

Percentage of the requests served within a certain time (ms)
  50%    768
  66%   1088
  75%   1451
  80%   1710
  90%   2312
  95%   3328
  98%   3388
  99%   3976
 100%   3976 (longest request)
```

```bash
# -g 开启调用关系分析，-p 指定 php-fpm 的进程号 21515
$ perf top -g -p 21515
```

![](perf.jpg)


```bash
# 停止原来的应用
$ docker rm -f nginx phpfpm
# 运行优化后的应用
$ docker run --name nginx -p 10000:80 -itd feisky/nginx:cpu-fix
$ docker run --name phpfpm -itd --network container:nginx feisky/php-fpm:cpu-fix
```

不知为何在bj机器上测试rps依然不足10，使用`time curl ...`得到的结果经常usr大于1s，甚至有5s+的，换到sgp上测试结果如下

```
ykg@sgp:~$ ab -c 10 -n 10000 http://13.67.110.15:10001/
This is ApacheBench, Version 2.3 <$Revision: 1807734 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 13.67.110.15 (be patient)
Completed 1000 requests
Completed 2000 requests
Completed 3000 requests
Completed 4000 requests
Completed 5000 requests
Completed 6000 requests
Completed 7000 requests
Completed 8000 requests
Completed 9000 requests
Completed 10000 requests
Finished 10000 requests


Server Software:        nginx/1.15.6
Server Hostname:        13.67.110.15
Server Port:            10001

Document Path:          /
Document Length:        9 bytes

Concurrency Level:      10
Time taken for tests:   3.722 seconds
Complete requests:      10000
Failed requests:        0
Total transferred:      1720000 bytes
HTML transferred:       90000 bytes
Requests per second:    2686.92 [#/sec] (mean)
Time per request:       3.722 [ms] (mean)
Time per request:       0.372 [ms] (mean, across all concurrent requests)
Transfer rate:          451.32 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    1   0.5      1       8
Processing:     1    2   0.8      2      13
Waiting:        1    2   0.8      2      13
Total:          2    4   0.9      4      14

Percentage of the requests served within a certain time (ms)
  50%      4
  66%      4
  75%      4
  80%      4
  90%      5
  95%      5
  98%      6
  99%      7
 100%     14 (longest request)
```

