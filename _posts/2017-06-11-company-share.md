---
layout: post
title: 内部分享Markdown、git-flow
share: true
comments: true
imagefeature:
tags: [markdown,git]
category: markdown,git
description: "提高质量、效率为目的的分享总结"
---



<!--more-->

## 环境准备

1. 最近更新到了MACOS 10.12.5，使用`bundle exec jekyll serve`启动本地博客提示无法找到bundle,原因是因为10.11系统开启的保护功能，在不关闭此功能下目前只能重新安装`sudo gem install bundler -n /usr/local/bin`需要的组件.
2. `npm install -g nodeppt`安装[nodeppt](https://github.com/ksky521/nodePPT)使用Markdown生成演示文档


## Markdown介绍

* Markdown 是一种轻量级标记语言 

		类似于其他标记语言是由格式+内容生成展示页面
	
* 以纯文本的形式发布,可转换成有效的XHTML(或者HTML)文档

		便于编辑、传输，任何一种文本编辑器都可以编写，展示需要使用模板样式

* 能让文档更容易读、写和随意改

		原始文档更易读写、与前端HTML相比简化很多
		
	![][1]



# Markdown使用场景

* 内容分享社区（wiki,简书,博客等）

	![][2]
	![][3]
	![][4]
	
* Peoject README

	![][6]
	
* 工作笔记

	![][5]
	

	


# Markdown基础样式
----

> Talk is cheap. Show me the code.



# git-flow

* 一套使用git的开发规范

[git-flow备忘清单]([code](https://danielkummer.github.io/git-flow-cheatsheet/index.zh_CN.html)




## 分支(branch)

* `develop`开发分支
* `feature/XXX`新特性分支
* `release`即将发布分支
* `master`稳定版本分支
* `hotfix/XXX`线上bug修改分支



## Tips
----

* 提交信息，打标签TASK ISSUE MODIFY ADD REMOVE... 

	很多项目提交是提交信息过于简单，无法起到版本跟踪，一目了然本地提交内容，建议在提交信息中添加标签，如使用需求、问题跟踪系统还可直接标注需求、问题编号等，方便多版本管理、切换
	
* .gitignore

	与项目无关文件（操作系统、IDE生成的临时文件）,本地编译文件（target/class等），本地配置等
	
* 分支太多/分支太少

	分支太多大多时候没有很好的管理好开发节奏，没有合理规划分支的使用及合并，分支太少对分布式版本控制了解的不够，把git当svn用

[1]: {{ site.url }}/images/posts/2017-06/share_1.png "share_1.png"
[2]: {{ site.url }}/images/posts/2017-06/share_2.png "share_2.png"
[3]: {{ site.url }}/images/posts/2017-06/share_2.png "share_3.png"
[4]: {{ site.url }}/images/posts/2017-06/share_2.png "share_4.png"
[5]: {{ site.url }}/images/posts/2017-06/share_2.png "share_5.png"
[6]: {{ site.url }}/images/posts/2017-06/share_2.png "share_6.png"

