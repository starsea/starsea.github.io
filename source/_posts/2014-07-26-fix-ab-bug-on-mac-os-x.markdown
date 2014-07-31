---
layout: post
title: "fix-a-ab-bug-on-mac-os-x"
date: 2014-07-26 16:27:23 +0800
comments: true
categories: linux
---

在 mac 下用 ab 压东西只要并发大了经常报错

```
apr_socket_recv: Connection reset by peer (54)
```
好像是 ab 的 bug 那我就重新编译一个吧

<!--more-->

```bash
cd ~/opt
wget http://apache.mirrors.pair.com/httpd/httpd-2.4.10.tar.gz
tar zxvf httpd-2.4.10.tar.gz
cd httpd-2.4.10
./configure
```
编译 apache 的时候会碰到下面情况
```
checking for APR-util... yes
checking for gcc... /Applications/Xcode.app/Contents/Developer/Toolchains/OSX10.9.xctoolchain/usr/bin/cc
checking whether the C compiler works... no
configure: error: in `/Users/haidx/opt/httpd-2.4.10':
configure: error: C compiler cannot create executables
See `config.log' for more details
```
厄..ls 查看了下 这个目录根本不存在嘛 所以哪里来的 gcc..扯犊子

查了下[资料](https://code.google.com/p/modwsgi/issues/detail?id=312)发现是 os x 10.8->10.9 升级后 xcode 路径不一致

解决方案:
```bash
$ cd /Applications/Xcode.app/Contents/Developer/Toolchains/
$ sudo ln -s XcodeDefault.xctoolchain OSX10.9.xctoolchain
```
然后编译通过了. done.
```
cd support
make
./ab -c 100 -n 10000 "url"
```

(update: 2014.07.31)

ps: 其实还是有问题...系统内核不知道怎么调..不说了都是泪还是别再 mac 下压吧...

参考:
* http://www.jsxubar.info/apache-ab-test-error.html



