---
layout: post
title: "use vagrant build linux environment to develop on mac os x"
date: 2014-07-06 00:51:03 +0800
comments: true
categories: mac
---

####vagrant 常用命令

```bash
$ vagrant box add {your_box_name} {mirror_uri}#我一般用先用迅雷下好
$ cd {your_destination_directory} #一个目录初始化一台虚拟机
$ vagrant init {your_box_name} #初始化
$ vagrant up  # 启动虚拟机
$ vagrant halt  # 关闭虚拟机
$ vagrant reload  # 重启虚拟机
$ vagrant ssh  # SSH 登录虚拟机
$ vagrant status  # 查看虚拟机运行状态
$ vagrant destroy  # 销毁当前虚拟机
```
ps: `vagrant init`不加 box 的名字 默认会初始化一个叫 base 的 box,但是其实 base box 是不存在的..这个时候需要把当前目录删掉 重新来过.

####关于 box 的一些操作
``` bash
[haidx@mbp:~/vagrant/centos64_test]$ vagrant box --help
Usage: vagrant box <subcommand> [<args>]

Available subcommands:
     add
     list
     outdated
     remove
     repackage
     update

For help on any individual subcommand run `vagrant box <subcommand> -h`

[haidx@mbp:~/vagrant/centos64_test]$ vagrant box list
centos64     (virtualbox, 0)
centos64test (virtualbox, 0)
[haidx@mbp:~/vagrant/centos64_test]$ vagrant box remove centos64test
Removing box 'centos64test' (v0) with provider 'virtualbox'...

```