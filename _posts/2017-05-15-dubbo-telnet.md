---
layout: post
title: 使用telnet调试dubbo接口
share: true
comments: true
imagefeature:
tags: [java,linux]
category: java
description: "dubbo服务调试"
---



<!--more-->

## dubbo端口连接

使用配置中心或者直接查看代码配置中具体服务提供方的IP及port，使用telnet连接

```shell
telnet 192.168.220.164 3000

```

![ ][1]

## 常用命令

`ls` 显示服务列表

`ps` 显示服务端口列表

`cd` 改变缺省服务

![ ][2]

`trace` 跟踪服务调用情况

`count` 统计服务任意方法的调用情况

`invoke`服务调用，复杂对象采用json格式传入

`status`dubbo服务状态，当全部OK时则显示OK，只要有一个ERROR则显示ERROR，只要有一个WARN则显示WARN

`log`改变dubbo日志级别 *log 1000* 查看log最后1000个字符


参考：

[Telnet命令参考手册](http://dubbo.io/Telnet+Command+Reference-zh.htm)




[1]: {{ site.url }}/images/posts/2017-05/dubbo-telnet-1.png "dubbo-telnet-1"
[1]: {{ site.url }}/images/posts/2017-05/dubbo-telnet-2.png "dubbo-telnet-2"
