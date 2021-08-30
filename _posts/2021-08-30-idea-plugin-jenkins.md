---
layout: post
title: idea集成jenkins
share: true
comments: true
imagefeature:
tags: [develop,idea,jenkins]
category: develop,ide,idea
description: "idea集成jenkins"
---



<!--more-->

## idea安装jenkins插件

![][1]

## 插件配置

![][2]

### 低版本


http://10.1.90.50:8111/jenkins/configureSecurity/

![][3]

从 http://10.1.90.50:8111/jenkins/crumbIssuer/api/xml?tree=crumb 获取Crumb Data 


### 高版本

jenkins运行参数中添加

> -Djava.awt.headless=true -Dhudson.security.csrf.GlobalCrumbIssuerConfiguration.DISABLE_CSRF_PROTECTION=true

Crumb Data 留空

此时再进入  http://10.1.90.50:8111/jenkins/configureSecurity/ 会出现以下提示

![][4]

## 调用
![][5]


### 注意
http://10.1.90.50:8111/jenkins/configure

![][6]

[1]:{{ site.url }}/images/posts/2021-08-31/1.png "1"

[2]:{{ site.url }}/images/posts/2021-08-31/2.png "2"

[3]:{{ site.url }}/images/posts/2021-08-31/3.png "3"

[4]:{{ site.url }}/images/posts/2021-08-31/4.png "4"

[5]:{{ site.url }}/images/posts/2021-08-31/5.png "5"

[6]:{{ site.url }}/images/posts/2021-08-31/6.png "6"

