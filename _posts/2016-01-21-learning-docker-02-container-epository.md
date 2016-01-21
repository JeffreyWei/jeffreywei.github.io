---
layout: post
title: Docker(2)-容器&仓库
share: true
comments: true
imagefeature:
tags: [Docker]
category: Docker
description: "Docker Container&Repository"
---

Docker容器、仓库的相关操作

<!--more-->
#容器

##创建容器

	docker create
	
创建出的容器处于停止状态，可以使用docker statrt启动，如果需要创建并启动容器使用

	docker run
	
##进入容器

使用`-d`启动的容器会进入后台，使用如下命令可以连接后台的容器

	docker attach 

##删除容器

	docker rm
	
##导入、导出容器
导出一个已创建的容器到一个文件

	docker export CONTAINER
	docker import CONTAINER
		
		
		
#仓库
##创建私有仓库

使用官方的`registry`镜像搭建本地私有仓库

	docker run -d -p 5000:5000 -v /temp/dockerregistry:/tmp/registry registry
	
##上传镜像到私有仓库

	docker push localhost:5000/IMAGE 

	