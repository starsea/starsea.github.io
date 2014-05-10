---
layout: post
title: "Git 阅读笔记(一)"
date: 2014-05-09 11:18:15 +0800
comments: true
categories: Git
---
设置参数
`git config —global  user.name 'starsea'`  

- system 编辑 /etc/gitconfig
- global 编辑 ~/.gitconfig
- 不带参数 编辑 .git/config 

以上依次覆盖配置参数

本篇文章将记录  `git config diff igonre add` 等基础命令.
<!--more-->

##git add 
>这是个多功能命令，根据目标文件的状态不同，此命令的效果也不同：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等

如果已经`git add`一个文件了，再对它进行修改 那么运行`git status`会发现该文件出现**两次**实际上 Git 只不过暂存了你运行 `git add` 命令时的版本，如果现在提交，那么提交的是添加注释前的版本，而非当前工作目录中的版本。所以，运行了 `git add` 之后又作了修订的文件，需要重新运行 `git add` 把最新版本重新暂存起来：


##gitignore

####规范：

- 所有空行或者以注释符号 `＃` 开头的行都会被 Git 忽略。
- 可以使用标准的 `glob` 模式匹配。
- 匹配模式最后跟反斜杠（`/`）说明要忽略的是目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（`!`）取反。

####什么是glob模式

>所谓的 `glob` 模式是指 shell 所使用的简化了的正则表达式。星号（*）匹配零个或多个任意字符；[abc] 匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；问号（?）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 [0-9] 表示匹配所有 0 到 9 的数字）。


	# 此为注释 – 将被 Git 忽略
	# 忽略所有 .a 结尾的文件
	*.a
	# 但 lib.a 除外
	!lib.a
	# 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
	/TODO
	# 忽略 build/ 目录下的所有文件
	build/
	# 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
	doc/*.txt
	# ignore all .txt files in the doc/ directory
	doc/**/*.txt
	
##git diff

- `git diff` 比较当前文件和暂存区的差异
- `git diff --cached` 比较暂存区和仓库里的快照差异 


##获取配置

	[haidx@mbp:~/www/cmp]$ git config --list
	user.email=xxx@gmail.com
	user.name=starsea
	core.excludesfile=/Users/haidx/.gitignore
	core.autocrlf=input
	alias.lg=log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --
	core.repositoryformatversion=0
	core.filemode=true
	core.bare=false
	core.logallrefupdates=true
	core.ignorecase=true
	core.precomposeunicode=false
	remote.origin.url=git@github.com:starsea/cmp.git
	remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
	branch.master.remote=origin
	branch.master.merge=refs/heads/master



##获取本地配置

	[haidx@mbp:~/www/cmp]$ git config --local --list
	core.repositoryformatversion=0
	core.filemode=true
	core.bare=false
	core.logallrefupdates=true
	core.ignorecase=true
	core.precomposeunicode=false
	remote.origin.url=git@github.com:starsea/cmp.git
	remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
	branch.master.remote=origin
	branch.master.merge=refs/heads/master
	
	
	[haidx@mbp:~/www/cmp]$ cat .git/config
	[core]
	     repositoryformatversion = 0
	     filemode = true
	     bare = false
	     logallrefupdates = true
	     ignorecase = true
	     precomposeunicode = false
	[remote "origin"]
	     url = git@github.com:starsea/cmp.git
	     fetch = +refs/heads/*:refs/remotes/origin/*
	[branch "master"]
	     remote = origin
	     merge = refs/heads/master



	
	
	
	



