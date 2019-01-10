---
layout: post
title: "How Twitter Uses Redis To Scale - 105TB RAM, 39MM QPS, 10,000+ Instances"
date: 2014-09-19 15:32:11 +0800
comments: true
categories: redis
---
第一次翻译文章..见谅..

Yao Yue 2010年加入 twitter 的缓存开发团队,她最近做了一场关于 redis 课程的精彩演讲: [Scaling Redis at Twitter.](https://www.youtube.com/watch?v=rP9EKvWt0zo) 但不仅仅是 redis.

Yao 已经在 twitter 工作几年了.她经历过很多事情. 她见证了缓存服务在 twitter 从一个项目到近上百个项目被使用的爆炸式增长. 成千上万的机器, 庞大的集群, TB 级的内存.

It's clear from her talk that's she's coming from a place of real personal experience and that shines through in the practical way she explores issues. It's a talk well worth watching.

正如你所预料的 twitter 使用了大量的缓存

时间轴服务为一个数据中心使用 Hybrid List:
* ~40TB allocated heap
* ~30MM qps
* &gt; 6,000 instances

在一个数据中心使用 BTree:
* ~65TB allocated heap
* ~9MM qps
* &gt; 4,000 instances

You'll learn more about BTree and Hybrid List later in the post.

两个要点: