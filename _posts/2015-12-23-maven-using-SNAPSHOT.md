---
layout: post
title: maven引用snapshot版本jar
share: true
comments: true
imagefeature:
tags: [java,maven]
category: java,maven
description: "using maven dependency snapshot"
---


<!--more-->
##问题描述

在本地编译上传snapshot jar,在其他环境无法正常获取，发现每次执行`mvn deploy`时都会在本地仓库生成相应的文件，本地编译其他依赖项目是没有问题的，但在其他环境编译发现始终从PublicRepositorie获取，推断部署阶段应该是没问题的，猜测问题出在snapshot获取的路径配置上，查看maven配置`setting.xml`。


##配置setting.xml
###mirror段
{% highlight xml %}
    <mirror>
        <id>snapshot</id>
        <mirrorOf>snapshots-repo</mirrorOf>
        <name>snapshots</name>
        <url>http://xxx.com/content/repositories/snapshots</url>
    </mirror>

{%  endhighlight %}
###profile段
{% highlight xml %}
    <profile>
     <id>allow-snapshots</id>
        <activation><activeByDefault>true</activeByDefault></activation>
     <repositories>
       <repository>
         <id>snapshots-repo</id>
         <url>http://xxx.com/content/repositories/snapshots</url>
         <releases><enabled>false</enabled></releases>
         <snapshots><enabled>true</enabled></snapshots>
       </repository>
     </repositories>
   </profile>
 {%  endhighlight %}
 
 再次编译依赖项目即可从SnapshotsRepositorie获取快照版本的类库。
 