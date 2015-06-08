---
layout: post
title: ngrok使用
share: false
comments: true
imagefeature:
tags: [internet,work]
category: internet,wrok
description: "using ngrok"
---

使用国内ngrok服务器实现网路服务映射

<!--more-->

##ngrok介绍

实现本地服务对外网域名映射，用来演示、开发部署在内网的系统，原理是http、https协议转发。

比如本地有一个web项目：`127.0.0.1:8080`只能在内网环境中访问，使用ngrok可以生成一个外网域名(例如`http://705b1117.ngrok.io/`)对本地地址进行映射，允许外网用户使用域名(http://705b1117.ngrok.io/)访问本地服务。


目前[官网]((https://ngrok.com)提供下载的版本为2.0+暂时不能指定第三方服务器，命令上也有些变化，使用自定义服务器需要使用ngrok 1.7，下载地址：


[ngrok_for_linux](https://ngrokd.b0.upaiyun.com/clients/ngrok_for_linux.zip)

[ngrok_for_win](https://ngrokd.b0.upaiyun.com/clients/ngrok_for_win.zip )

[ngrok_for_macosx](https://ngrokd.b0.upaiyun.com/clients/ngrok_for_macosx.zip )

[ngrok_for_win_64bit](https://ngrokd.b0.upaiyun.com/clients/ngrok_for_win_64bit.zip)


##配置ngrok 1.7

1. 编辑配置文件，保存以下内容为`ngrok.cfg`:
	
	
		server_addr: "tunnel.mobi:44433"
		trust_host_root_certs: true
	此配置使用国内服务器，速度上比默认的国外服务器会快一些。

2. 执行:
	
	{% highlight bash lineno  %}
	./ngrok -config ngrok.cfg -subdomain domainname 8080
	{%  endhighlight %}

##使用ngrok 2.0+
  
执行:

{% highlight bash lineno  %}
$ ./ngrok -proto=http 8080
{%  endhighlight %}

