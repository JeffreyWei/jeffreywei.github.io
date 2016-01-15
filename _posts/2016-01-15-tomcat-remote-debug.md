---
layout: post
title: tomcat远程调试
share: true
comments: true
imagefeature:
tags: [java]
category: java
description: "tomcat remote debug"
---

配置使用tomcat远程调试功能，使用idea远程调试项目

<!--more-->
##tomcat配置
开启tomcat远程配置的方式有多种，下面介绍其中的一种。

1. 检查环境变量`CATALINA_BASE`，指向tomcat根目录;
2. 在`bin`目录中创建文件`setenv.sh`，windows环境命名为`setenv.bat`;
3. 在上面的文件中添加如下命令:
	
		EXPORT CATALINA_OPTS="-agentlib:jdwp=transport=dt_socket,address=8000,server=y,suspend=n"

	windwos

		SET CATALINA_OPTS="-agentlib:jdwp=transport=dt_socket,address=8000,server=y,suspend=n"

	此处指定的远程调试端口为`8000`。
##IDE配置
这里使用的是`idea`，在`Run/Debug Configurations`中创建类型为`remote`的应用，输入部署tomcat主机的ip:port选择本地源码所属的模块，启动`debug`连接远程tomcat即可远程调试项目。
##tomcat&jetty
最近在工作中遇到的一个部署问题，是由于URL编码造成的，开发环境使用maven插件使项目运行在jetty上，线上环境为tomcat，部署后发现前台传入的中文无法正常到达后台，查看tomcat配置文档，需要在server.xml中配置连接的编码格式，添加`URIEncoding="UTF-8"`配置。