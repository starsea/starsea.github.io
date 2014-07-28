---
layout: post
title: "在本地用 Satis 搭建 Composer Packages 镜像"
date: 2014-05-16 10:35:27 +0800
comments: true
categories: composer
---
这篇文章将教导你怎么在本地搭建一个`composer`镜像以节约大量的网络开销时间.

##What Is Satis?

`Sitis`是我们将要用来对我们的项目镜像各种仓库的应用程序名字.它就像一个代理坐落在 Internet 和你的 Composer 之间. 我们的解决方案将要创建一个本地的一些包的镜像并且通知我们的 composer 来使用它来替代在 Internet 上寻找 sources.

一张图胜过千言万语.

{% img /images/posts/composer.png %}


如果在本地找到了包,那么讲从本地安装,如果没有,那么将默认从 `packagist.org` 安装.
<!--more-->
##Getting Satis

`Satis` 安装非常简单, 直接通过 composer 安装.首先安装 composer.
```bash
curl -sS https://getcomposer.org/installer | php
```
然后安装 satis
```bash
[haidx@mbp:~/repo/test]$ composer  create-project composer/satis --stability=dev --keep-vcs
Installing composer/satis (dev-master ef2a73a9c4250abcde4349d3d468d3cb5286c529)
  - Installing composer/satis (dev-master master)
    Cloning master

Created project in /Users/haidx/repo/test/satis
Loading composer repositories with package information
Installing dependencies from lock file
  - Installing symfony/process (dev-master 0ae3bf5)
    Cloning 0ae3bf5ff90edbe6e30e645221646479c2f1ef54

  - Installing symfony/finder (v2.4.2)
    Downloading: 100%

  - Installing symfony/console (dev-master afee085)
    Cloning afee08520a971c0f073bff89e1d456405e244c37

  - Installing seld/jsonlint (1.1.2)
    Downloading: 100%

  - Installing justinrainbow/json-schema (1.1.0)
    Downloading: 100%

  - Installing composer/composer (dev-master 0d4c2bb)
    Cloning 0d4c2bb7d7a864a9b3e876908e743310cdeaa5e6

  - Installing twig/twig (v1.15.1)
    Loading from cache

symfony/console suggests installing symfony/event-dispatcher ()
Generating autoload files
```
//todo 未完待续



##参考文献
- http://moquet.net/blog/proxify-composer-php/
- http://melp.nl/2013/09/composer-create-a-local-package-repository-to-improve-speed/
- http://code.tutsplus.com/tutorials/setting-up-a-local-mirror-for-composer-packages-with-satis--net-36726
- http://getcomposer.ycnets.com/doc/articles/handling-private-packages-with-satis.md