---
layout: post
title: "install octopress on mac os x 10.9"
date: 2014-05-09 14:53:37 +0800
comments: true
categories: octopress
---

安装方法就不多说了 详情见参考文献 说下注意初学者难理解的地方

整个项目分为两个部分 一部分是博客的源码 也就是source分支(除开`_deploy`目录之外的其他所有文件)  
还有一部分是网站的静态资源(打开网站你看到的全部都是静态资源 所有的页面都是纯html页面) 这部分属于master分支 也就是`_deploy`整个目录 你可以这样看看

	[haidx@mbp:~/repo/octopress]$ cd _deploy/
	[haidx@mbp:~/repo/octopress/_deploy]$ git status
	# On branch master
	nothing to commit, working directory clean
	[haidx@mbp:~/repo/octopress/_deploy]$
		

安装好之后 运行 `rake new_post["myTitle"]` 会自动生成一个 markdown 文件

文章生成在目录下的`source/_posts`目录下。文章是markdown格式的。可以通过 `Mou` 软件来编辑保存。

`rake generate` 根据 `_config.yml` 和 `source/_posts`下的 markdown 文件生成网站所有需要的静态资源.

`rake deploy` 这个命令是把网站所有生成的静态资源放到 _deploy 目录下,然后把该目录下得所有文件推送到github的 master 分支.

发布之后运行`git status`之后发现还是有很多修改文件 , 没错 因为 `rake deploy ` 并没有 push source 分支过去. 如有需要 可以手动 push

	git add .
	git commit -m 'Initial source commit'
	git push origin source


[参考]

- [zhuan-zai-zai-github-shang-shi-yong-octopress](http://yang3wei.github.io/blog/2013/01/28/zhuan-zai-zai-github-shang-shi-yong-octopress/)
- [install-ocotpress-on-mac-and-host-website-on-github](http://www.ikitweb.com/blog/2014/04/11/install-ocotpress-on-mac-and-host-website-on-github/)
- [像黑客一样写博客](http://blog.csdn.net/jackystudio/article/details/16353865)
- http://blog.segmentfault.com/yaashion_xiang/1190000000364677
- http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/
