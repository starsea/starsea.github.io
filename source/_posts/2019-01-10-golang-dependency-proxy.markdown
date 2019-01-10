---
layout: post
title: "Golang 拾遗（一）之依赖处理"
date: 2019-01-10 19:39:53 +0800
comments: true
categories: golang
---


##Git Clone

`git clone ssh` 调用的是 `ssh` 

`git clone http` 调用的是 `git-remote-http` 

`git clone https` 调用的是 `git-remote-https` 

## Go Get
go get 会把库安装到Gopath 下

比如我的GOPATH 是 `~/work/xinyue/go`  执行命令 `go get github.com/BurntSushi/toml`

那么库会被安装到 `~/work/xinyue/go/src/github.com/BurntSushi/toml`

<!-- more -->

###本质
Go Get 本质是调用git 我们可以通过制定git配置的代理来配置一些东西

vim ~/.gitconfig

``` 
#Demo
[http "https://domain.com"]
	proxy = http://proxyUsername:proxyPassword@proxy.server.com:port
	sslVerify = false
	
[http "https://git.code.oa.com"]
      proxy = http://127.0.0.1:12639
[http "http://git.code.oa.com"]
      proxy = http://127.0.0.1:12639

```

有时候我们笔记本电脑有时候在家里办公 有时候在公司办公 需要经常切换网络环境 如果每次来还切换配置文件就很麻烦 幸好我们有可视化的软件 `proxifier`

###自动配置
`Go Get` 调用`Git`  `Git` 其实调用的是 `git-remote-https` 这个二进制程序去抓取远程内容 我们配置规则即可

![](media/15468459121385/15471079183836.jpg)

如上所示 我们配置了两行规则 第一行当调用git.code.oa.com 的时候 走内网代理 其他的全部是走外网代理。



##Glide
Glide 是Go用的最多的依赖管理工具 这里不做更多赘述

唯一需要注意的是glide 默认用的是https 有时候我们访问一些仓库如果用https（比如公司的内网仓库）需要手动输入密码  我们希望指定能通过ssh免密

可以通过git修改 ( 因为glide拉取git类型仓库的时候还是会去调用git )

```
git config --global url."user@git.private.com:".insteadOf "https://git.private.com"
```

或者我们直接修改glide.yaml 文件 指定访问方式 比如

```
package: native
import:
- package: git.code.oa.com/up-common/go-http-core
  repo: git@git.code.oa.com:up-common/go-http-core.git
  vcs: git
  subpackages:
  - middleware/jce
- package: github.com/BurntSushi/toml
  version: ~0.3.1
- package: github.com/gin-gonic/gin
  version: ~1.3.0
- package: github.com/mcuadros/go-version
- package: gojce
  repo: git@git.code.oa.com:up-common/gojce.git
  vcs: git
```

我们所想Glide的代理怎么配置 //todo

##参阅

* https://gist.github.com/evantoli/f8c23a37eb3558ab8765 git proxy
* https://github.com/Masterminds/glide/wiki/Go-Package-Manager-Comparison Go包管理
* https://github.com/Masterminds/glide/issues/836 glide ssh


