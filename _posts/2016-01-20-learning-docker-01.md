---
layout: post
title: Docker(1)-镜像
share: true
comments: true
imagefeature:
tags: [Docker]
category: Docker
description: "Docker mirror"
---

Docker镜像的相关操作

<!--more-->

##镜像获取

	docker pull [DockerRepositorieAddress/]IMAGENAME[:TAG]
	
* DockerRepositorieAddress:用于指定镜像来源的仓库地址，默认镜像仓库为Docker Hub
* TAG:用于指定镜像版本，如不显示指定，默认会选择latest作为镜像的标签，获取最新版本的镜像

镜像下载时会以层的方式进行，层(Layer)其实是AUFS(Advanced Union File System,一种联合文件系统)中的重要概念，是实现增量保存与更新的基础。

##镜像信息查看

1. 查看本机已下载的镜像

		docker images
	
	此命令会显示一下信息：

	* 来源的仓库地址
	* 镜像的标签
	* 镜像的ID(唯一)
	* 创建时间
	* 镜像大小


2. 为镜像指定标签

		docker tag [DockerRepositorieAddress/]IMAGENAME:TAG IMAGES:NEWTAG
	
	使用docker images命令查看，新命名的标签和旧的标签指向的是同一个镜像ID，他们只是别名不同。

3. 查看镜像详细信息

		docker inspect IMAGEID
		
	*在指定IMAGEID时，通过使用该ID前几个字符即可*
		
##镜像搜索

	docker search TERM
	
	
##删除镜像

	docker rmi IMAGE[IMAGE...]
	
删除时也可是使用`镜像ID`来确定需要删除的镜像。如果镜像正在被使用使用rmi是无法删除的，可以使用`docker rmi -f`强制删除，但不建议这么做，需要先使用`docker rm`删除依赖的容器然后再删除镜像。

##创建镜像

目前创建镜像的方式由三种：

* 基于现有镜像的容器创建
* 基于本地模板的导入
* 基于Dockerfile创建

下面介绍相对容易理解的前两种
	
1. 基于现有镜像的容器创建
	
		docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
	
	例如，对Ubuntu镜像进行操作后创建新的镜像
	
		docker commit -m "提交信息" -a '作者信息' ORIGINIMAGEID NEWIMAGE

2. 基于本地模板的导入

	使用操作系统模板文件导入一个镜像文件
	
		cat centos-xxx.xx.x.tar.gz 	| docker import - centos:tag




##保存和载入镜像

1. 保存镜像

	导出镜像为本地文件
		
		docker save -o centos.tar centos:latest
	

2. 载入镜像
	
	使用本地文件生成镜像
	
		docker load --input centos.tar 
		或
		docker load < centos.tar


##上传镜像

	docker push NAME[:TAG]
		
		
		
		
		
		
		
		
		
		
		
