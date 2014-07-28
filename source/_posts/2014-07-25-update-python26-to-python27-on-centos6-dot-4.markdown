---
layout: post
title: "update-python26-to-python27-on-centos6.4"
date: 2014-07-25 22:04:13 +0800
comments: true
categories: 
---

系统自带的 python...

```bash
md5sum /usr/bin/python2.6
md5sum /usr/bin/python

wget http://python.org/ftp/python/2.7.3/Python-2.7.3.tar.bz2
tar -jxvf Python-2.7.3.tar.bz2
cd Python-2.7.3
./configure
make
sudo make install
```

这里有个好消息就是 centos6.4 并不会把 /usr/local/python 指向 /usr/local/bin/python 记得在上家公司 centos5.x 版本的时候是会产生这个问题的 从而导致 yum 工作(yum 只能在特定版本下工作 5.x 是 py2.4 6.4是 py2.6).

```bash
[vagrant@localhost ~]$ ll /usr/local/bin
总用量 6056
-rwxrwxr-x 1 root root     101 7月  25 13:52 2to3
-rwxrwxr-x 1 root root      99 7月  25 13:52 idle
lrwxrwxrwx 1 root root      37 7月  17 03:01 nginx -> /usr/local/webserver/nginx/sbin/nginx
-rwxrwxr-x 1 root root      84 7月  25 13:52 pydoc
lrwxrwxrwx 1 root root       7 7月  25 14:00 python -> python2
lrwxrwxrwx 1 root root       9 7月  25 14:00 python2 -> python2.7
-rwxr-xr-x 1 root root 6162265 7月  25 14:00 python2.7
-rwxr-xr-x 1 root root    1624 7月  25 14:00 python2.7-config
lrwxrwxrwx 1 root root      16 7月  25 14:00 python2-config -> python2.7-config
lrwxrwxrwx 1 root root      14 7月  25 14:00 python-config -> python2-config
-rwxrwxr-x 1 root root   18547 7月  25 13:52 smtpd.py
[vagrant@localhost ~]$
[vagrant@localhost ~]$ ll /usr/local/bin
总用量 6056
-rwxrwxr-x 1 root root     101 7月  25 13:52 2to3
-rwxrwxr-x 1 root root      99 7月  25 13:52 idle
lrwxrwxrwx 1 root root      37 7月  17 03:01 nginx -> /usr/local/webserver/nginx/sbin/nginx
-rwxrwxr-x 1 root root      84 7月  25 13:52 pydoc
lrwxrwxrwx 1 root root       7 7月  25 14:00 python -> python2
lrwxrwxrwx 1 root root       9 7月  25 14:00 python2 -> python2.7
-rwxr-xr-x 1 root root 6162265 7月  25 14:00 python2.7
-rwxr-xr-x 1 root root    1624 7月  25 14:00 python2.7-config
lrwxrwxrwx 1 root root      16 7月  25 14:00 python2-config -> python2.7-config
lrwxrwxrwx 1 root root      14 7月  25 14:00 python-config -> python2-config
-rwxrwxr-x 1 root root   18547 7月  25 13:52 smtpd.py
```