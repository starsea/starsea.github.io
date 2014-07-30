---
layout: post
title: "CentOS yum 源的配置与使用"
date: 2014-07-28 21:03:08 +0800
comments: true
categories: linux
---


```bash
[root@localhost yum.repos.d]# rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
Retrieving http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
warning: /var/tmp/rpm-tmp.ZlOIWb: Header V3 DSA/SHA1 Signature, key ID 00f97f56: NOKEY
error: Failed dependencies:
	epel-release >= 6 is needed by remi-release-6.5-1.el6.remi.noarch
```
remi 源需要 epel-release(>6)...好吧看来还是要按照 epel 啊..

安装 epel
```bash
[root@localhost yum.repos.d]# rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
Retrieving http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
warning: /var/tmp/rpm-tmp.vpGlLN: Header V3 RSA/SHA256 Signature, key ID 0608b895: NOKEY
Preparing...                ########################################### [100%]
   1:epel-release           ########################################### [100%]
```

导入key：
```bash
rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6
```
再次安装remi
```bash
$ rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
$ rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-remi
```
关于设置级别事情 肯定要安装 priorities
```
yum install yum-priorities
```

首先我们要清楚 remi 源到底有什么东西 [这里](http://rpms.famillecollet.com/enterprise/6.4/remi)可以看到每个`repository entry`对应有哪些东西..点进去就知道了...我们基本上就是 php mysql..等相关东西..由于我安装这东西就是为了用最新版的 php(orz 虚拟机懒得编译了)

note:

优先级由 1 ~ 99 的 99 个数表示，1 的优先级最高。优先级小的源即使有某软件的较新版本，如果优先级高的源中没有，在启用该插件的情况下，系统也无法安装/升级到该较新版本。图形界面的 YUM 工具一般默认就已经包含了优先级插件。

这里我将 remi 和 remi-php55 都设置 `priority=1` base 设置为2 epel 设置为5

当然如果在没开启 remi 的情况(enable=0)下你也可以这么做
```
yum --enablerepo=remi-php55,remi install php
```


* [Priorities wiki](http://wiki.centos.org/PackageManagement/Yum/Priorities)
* http://cnzhx.net/blog/remi-repository/
* http://www.cnblogs.com/mchina/archive/2013/01/04/2842275.html
* [yum 详细安装](http://www.trackself.com/archives/2397.html)



