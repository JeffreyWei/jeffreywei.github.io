---
layout: post
title: centos7服务端口被拒绝
share: true
comments: true
imagefeature:
tags: [linux]
category: linux
description: "linux"
---

公司办公地点搬迁，冷启动服务器后一部分服务无法连接

<!--more-->

## 状况

右边异常，左边正常

![][1]

## 检查服务配置、端口

已zookeeper为例，
* 检查zoo.cfg，正常
* 启动后检查对应端口`netstat -lnp|grep 2181`，正常

## 检查系统防火墙配置

`sudo service iptables status`发现有一个firewall是开启的，网上查找后发现是centos7开启的防火墙，检查几台服务器，不正常的都是centos7,使用命令关闭后，服务恢复正常

## centos7-firewall

* systemctl start firewalld         # 启动
* systemctl enable firewalld        # 开机启动
* systemctl stop firewalld          # 关闭
* systemctl disable firewalld       # 取消开机启动


[1]: {{ site.url }}/images/posts/2017-07/centos7-firewall.png "centos7-firewall"

