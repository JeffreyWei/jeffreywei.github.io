---
layout: post
title: 安装mysql-5.7.16-osx10.11-x86_64
share: true
comments: true
imagefeature:
tags: [python]
category: python
description: ""
---

mysql-5.7.16-osx10.11-x86_64

<!--more-->
    
    
之前装过两次mysql，第一次使用dmg方式，安装简单，提供UI引导、配置，但删除的时候比较麻烦，第二种是用homebrew安装，根据版本不同变化比较大，下面介绍另外一种更‘绿色’的方式

## 安装

* 官方直接下载mysql-5.7.16-osx10.11-x86_64.tar解压
* 移动`sudo mv mysql-5.7.16-osx10.11-x86_64 /usr/local/mysql`
* 改权限`sudo chown -R root:wheel mysql`
* 进入mysql根目录执行`sudo scripts/mysql_install_db --user=mysql`开始安装

最后一步注意提示会生成root密码



## 启动、关闭

* 启动`sudo support-files/mysql.server start`
* 重启`sudo support-files/mysql.server restart`
* 关闭`sudo support-files/mysql.server stop`
* 状态`sudo support-files/mysql.server status`



## 连接

* `bin/mysql -u root -p`
* 修改密码`set password for root@localhost = password('<password>');`

为了方便管理在zsh中添加

	alias mysqlstart='/usr/local/mysql/support-files/mysql.server start'
	alias mysqlstop='/usr/local/mysql/support-files/mysql.server stop'
	alias mysqlstatus='/usr/local/mysql/support-files/mysql.server status'
	


