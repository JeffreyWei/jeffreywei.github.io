---
layout: post
title: mongodb用户授权
share: true
comments: true
imagefeature:
tags: [work,nosql]
category: work,nosql
description: "mongodb 3.X install and user authorization"
---

mongo安装、用户授权

<!--more-->

最近开发的项目中使用了mongodb3.0，这一也是第一次在实际项目中使用mongodb,项目初期开发并没有使用验证，在今天下午review中把这个遗留问题提上了议程，着手开始设置mongodb的用户权限。能找到的资料都是3.0之前的版本，只好老老实实看官方的文档，目前大部分mongodb的GUI客户端工具不能很好的兼容3.0版本，并不是所有人都喜欢对着shell开发或是要使用3.0的新功能，所以请大家注意自己选择的版本。

## 准备工作
1.	 mongodb下载
	
		[官方下载地址](https://www.mongodb.org/dl/osx/x86_64)
		brew install mongodb --with-openssl

	
2.	设置环境变量

		export PATH=<mongodb-install-directory>/bin:$PATH
		 
3.	在mongdb根目录下创建数据文件目录（`db/data`）、日志文件目录 (`da/log`)

## mongodb 3.0用户创建、授权

1. 使用非认证方式启动mongodb创建管理用户
	
		mongod --dbpath "db/data" --logpath "db/log/db.log"

	{% highlight bash lineno %}
	use admin
	db.createUser({
    	user: "admin",
    	pwd: "12345678",
    	roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
 	})
 	db.shutdownServer()
	{%  endhighlight %}
		
2.	使用认证方式启动mongodb

		mongod --dbpath "db/data" --logpath "db/log/db.log" --auth

	{% highlight bash lineno %}
	use admin
	db.auth("buru","12345678")
	{%  endhighlight %}
	
返回1表示成功，此时“admin”账号只有管理功能。


3.	使用用户库，创建用户

	{% highlight bash lineno %}
	use crs
	db.createUser({
	   	user: "crs",
		pwd: "12345678",
		roles: [
		    { role: "readWrite", db: "crs" },
			{ role: "read", db: "crs_bak" }
	]})
	db.auth("crs","12345678")
	{%  endhighlight %}
	
仍然是返回1，成功，此时就可以使用insert方法向“crs”库中添加数据了。
	
	
**注：目前Robomongo还不能使用授权方式访问3.0的mongodb，**
	
	

![][1]
	
	
[1]: http://jeffreywei.github.io/assets/posts/2015-05/robomogo_connect_error.png "robomogo_connect_error"
	







