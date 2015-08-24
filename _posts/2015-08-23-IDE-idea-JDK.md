---
layout: post
title: Intellij IDEA使用记录
share: true
comments: true
imagefeature:
tags: [java]
category: java
description: "java IDE"
---

记录IDE使用过程中遇到的问题、解决方案

<!--more-->
##项目编译环境调整

同时修改JDK、Language level、及Setting->Java compiler中的配置。


##JDK Language level
`JDK`：使用的编译版本

`Language level`：代码编译检查的最低版本，比如设置为5.0就不能使用6.0、7.0的特性代码

可以配置`Project`的默认配置，也可以精确到每个`Modules`

##代码中使用Tab指标符

![](1)



[1]: {{ site.url }}/assets/posts/2015-08/23-1.png "23-1.png"