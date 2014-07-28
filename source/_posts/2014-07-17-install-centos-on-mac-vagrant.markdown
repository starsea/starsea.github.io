---
layout: post
title: "centos 安装后需要做的很多事情"
date: 2014-07-17 00:21:23 +0800
comments: true
categories: linux
---

装上 vagrant 了.但是 centos 是 minimal 版的,所以需要做很多东西(不是 mini 版的也需要做很多事 囧rz).其实很多事情都是玩 vps 的时候做过的,但是人老了记性不好,每次都要 google. 所以我觉得还是记录下来比较好.

先添加源吧..啥也不说了 直接上[阿里云][aliyun]的开源镜像吧..网易萎了..
<!--more-->
```bash
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
cd /etc/yum.repos.d
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
yum clean all
yum makecache
```

###sudo command 找不到命令
编译 nginx 的时候把 nginx 放在了/usr/local/bin 下 用 sudo 执行的时候提示找到不到命令 很奇怪 切换到 root 去执行 nginx 却可以 看了下 root 的path 明明有包含这个路径...

查了下资料后发现是 sudo 的原因

我们使用 `sudo visudo` 可以看到

```bash
# Preserving HOME has security implications since many programs
# use it when searching for configuration files. Note that HOME
# is already set when the the env_reset option is enabled, so
# this option is only effective for configurations where either
# env_reset is disabled or HOME is present in the env_keep list.
#
Defaults    always_set_home

Defaults    env_reset
Defaults    env_keep =  "COLORS DISPLAY HOSTNAME HISTSIZE INPUTRC KDEDIR LS_COLORS"
Defaults    env_keep += "MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE"
Defaults    env_keep += "LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES"
Defaults    env_keep += "LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE"
Defaults    env_keep += "LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY"

#
# Adding HOME to env_keep may enable a user to run unrestricted
# commands via sudo.
#
# Defaults   env_keep += "HOME"

Defaults    secure_path = /sbin:/bin:/usr/sbin:/usr/bin
```
这里每次调用 sudo 应用的环境变量是..`/sbin:/bin:/usr/sbin:/usr/bin` 难怪找不到..

一个简单解决方案就是 `vi ~/.bashrc`
```bash
alias sudo="sudo env PATH=$PATH"
```

因为系统预装的 sudo 在编译时缺省使用了 --with-secure-path 参数，因此当前用户使用 sudo 时环境变量 $PATH 被覆盖，通过添加上面那行别名设置，就会在执行 sudo 时把当前的 $PATH 的值再套用上，达到想要的效果。

上面这种 alias 并不是最完美的方式 有时候我们可能会查看一些 sudo 的配置 比如 `sudo -l` 会翻译成 `sudo env PATH=$PATH -l` 但是这个命令是无效的...因为 -l 等参数必须放在env 前面..所以我们还是用`sudo visudo`手动修改 secure_path 的值把.

当然，最根本的解决办法还是重新编译安装 sudo，编译时不使用 --with-secure-path 参数即可。有兴趣可以试下

参阅:

* [充分发挥 sudo 的作用](http://www.ibm.com/developerworks/cn/aix/library/au-sudo/index.html)
* [sudo-command-not-found](http://pickerwengs.blogspot.com/2011/10/sudo-command-not-found.html)


[aliyun]: http://mirrors.aliyun.com/help/centos "阿里云开源镜像"


###locate: command not found
没有 locate...
```bash
# yum -y install mlocate
# updatedb
```

