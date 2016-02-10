---
layout: post
title: Docker(3)-项目部署
share: true
comments: true
imagefeature:
tags: [Docker]
category: Docker
description: "Deploy project with Docker"
---

项目中使用Docker发布、部署

<!--more-->
##生成镜像
使用`dockerfile`生成包含war的镜像，此处使用的jetty作为容器

```
FROM jetty:9-jre7
MAINTAINER wei@yizhen.com
COPY target/uther.war /var/lib/jetty/webapps/ROOT.war
VOLUME /home/jetty/.dubbo
EXPOSE 8080
EXPOSE 20885
```	

*由于项目中使用了dubbo，需要在用户根目录创建一个可写权限的文件夹;对外暴露需要使用的端口*

使用maven打包项目

```
mvn  clean compile war:war -Dmaven.test.skip=true -P dev
```

使用`docker build`生成镜像

```
sudo docker build -t imagename:tag dockerfilepath/
```

##启动镜像

使用`docker run`启动容器

```
sudo docker run -d -p 80:8080 -p 20885:20885 -v /log:/log --net=host --name proejct_0210 imagename:tag
```
*项目对外输出文件日志，使用了一个映射卷;net参数使dubbo向外暴露的接口使用和宿主机器同网段的地址*

##基础容器
###mysql

```
sudo docker run --name mysql -v /mysqldata:/var/lib/mysql -p 3306:3306
-e MYSQL_ROOT_PASSWORD=toor -d mysql:5.6
```

###jenkins

```
sudo docker run -it  -p 8080:8080 -p 50000:50000 -v /jenkinsdata:/var/
jenkins_home jenkins
```

###registry

```
sudo docker run -p 5000:5000 -v /docker_registry:/tmp/registry-dev  registry
sudo docker push 127.0.0.1:5000/imagename
```


###rancher

```
sudo docker run -d --restart=always -p 8080:8080 rancher/server
```






