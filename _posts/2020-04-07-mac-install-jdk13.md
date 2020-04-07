---
layout: post
title: mac10.14安装jdk13
share: true
comments: true
imagefeature:
tags: [java,mac]
category: java,mac
description: "mac10.14安装jdk13"
---



<!--more-->

## jdk下载

从速度比较快的华为下载对应版本jdk[https://repo.huaweicloud.com/java/jdk/](https://repo.huaweicloud.com/java/jdk/)，mac可使用的有两个版本

* dmg 和系统结合更友好
* archive  安装方式更"绿色"，IDE直接指向，与系统对接麻烦，mac对java命令都有包装，需要额外配置环境变量

以下采用dmg安装方式

## 多环境配置

打开本地shell配置文件，添加jdk13支持

{% highlight shell  %}	
export JAVA_7_HOME=`/usr/libexec/java_home -v 1.7`
export JAVA_8_HOME=`/usr/libexec/java_home -v 1.8`
export JAVA_13_HOME=`/usr/libexec/java_home -v 13`
export JAVA_HOME=$JAVA_13_HOME
alias jdk7="export JAVA_HOME=$JAVA_7_HOME"
alias jdk8="export JAVA_HOME=$JAVA_8_HOME"
alias jdk13="export JAVA_HOME=$JAVA_13_HOME"
{%  endhighlight %}

## 模式jdk调整

在多jdk环境下，系统默认会使用版本最高的jdk，如果需要设置默认版本，可以调整一下文件，trick如下：

** /Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Info.plist **里的JVMVersion的值由1.8.0_222改为 x1.8.0_222(大概第42行)。这样我们的adoptopenjdk-8.jdk就变成最新版本的 JDK 了。







