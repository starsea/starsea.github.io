---
layout: post
title: "ab 压力测试"
date: 2014-05-15 15:23:02 +0800
comments: true
categories: Linux
---
想必很多人都知道 `ab` 是干啥的,网上也有很多介绍用 `ab` 做压力测试的文章,但是大多对其结果介绍的都是稀里糊涂的.这里我们简单介绍下 `ab` .

##Usage

```bash
ab -n 5000 -c 200 http://thisobj.com/
```
这里表示总共对目标网址发起5000个请求 并发为200 那么也就是说 `一共会发起25次,每次并发200的请求.`

<!--more-->

##Run

```bash
[haidx@mbp:~/Documents]$ ab -n 5000 -c 200 http://thisobj.com/
This is ApacheBench, Version 2.3 <$Revision: 655654 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking thisobj.com (be patient)
Completed 500 requests
Completed 1000 requests
Completed 1500 requests
Completed 2000 requests
Completed 2500 requests
Completed 3000 requests
Completed 3500 requests
Completed 4000 requests
Completed 4500 requests
Completed 5000 requests
Finished 5000 requests


Server Software:        nginx/1.7.0
Server Hostname:        thisobj.com
Server Port:            80

Document Path:          /
Document Length:        16635 bytes

Concurrency Level:      200
Time taken for tests:   451.574 seconds
Complete requests:      5000
Failed requests:        232
   (Connect: 0, Receive: 0, Length: 232, Exceptions: 0)
Write errors:           0
Non-2xx responses:      232
Total transferred:      80371251 bytes
HTML transferred:       79357714 bytes
Requests per second:    11.07 [#/sec] (mean)
Time per request:       18062.978 [ms] (mean)
Time per request:       90.315 [ms] (mean, across all concurrent requests)
Transfer rate:          173.81 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:       71   98  64.6     79    1243
Processing:   316 17286 11229.2  13360   75174
Waiting:      244 17189 11246.3  13268   75174
Total:        389 17384 11230.9  13453   75299

Percentage of the requests served within a certain time (ms)
  50%  13453
  66%  15054
  75%  15717
  80%  17968
  90%  27504
  95%  44105
  98%  60217
  99%  60277
 100%  75299 (longest request)
```
这里最重要的就是这几个个参数了 `Requests per second` `Time per request`

>`Requests per second` 也就是我们常说的 `Rps` or `Qps` 表示每秒可以处理的请求个数.
这里有两个 Time per request 第一个表示**每一次**请求所需要的平均时间, 由于我们这里一次请求包含25个并发,这里一个并发的时间就是第二个参数.(cpu是分时间片轮流执行请求的，多并发的情况下，一个并发上的请求时需要等待这么长时间才能得到下一个时间片。)

所以我们可以发现
>rps = 1000 / (the first time pre request)

>the firest time pre requrest = the second time rpe request * 一个请求包含的并发数

###参考文献
- http://xinwo.acg.ac/sbc-and-qps/
- http://www.nginx.cn/110.html
- http://www.ha97.com/4617.html