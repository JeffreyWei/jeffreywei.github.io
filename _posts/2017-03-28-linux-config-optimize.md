---
layout: post
title: Linux服务器配置优化
share: true
comments: true
imagefeature:
tags: [scala,sbt]
category: bigdata
description: ""
---



<!--more-->


* 系统连接数

		ulimit -n 65535

	查看命令
	
		ulimit -a
		
* socket最大连接数

	sudo echo 10000 > /proc/sys/net/core/somaxconn
	
* 加快tcp回收系统默认是0。

	sudo echo  1 > /proc/sys/net/ipv4/tcp_tw_recycle
	
* 洪水抵御保护系统默认是1

		echo 0 > /proc/sys/net/ipv4/tcp_syncookies
	
	这个修改强烈建议在做完压力测试后改回来
	
* Nginx

	Nginx使用默认配置的话，只能接受1024个并发请求。我们可以通过修改配置来增加并发。找到nginx.conf，位置一般在/etc/nginx/nginx.conf。在Nginx全局的配置中worker_processes 1的下面加上worker_rlimit_nofile 10240，允许Nginx的子进程可以打开10240个文件。event中，worker_connections从1024改为10000。

	
	
```shell
ulimit -n 65535
sudo echo 10000 > /proc/sys/net/core/somaxconn
sudo echo  1 > /proc/sys/net/ipv4/tcp_tw_recycle

```