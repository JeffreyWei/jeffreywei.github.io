---
layout: post
title: 在Maven中配置Jetty使用https协议
share: true
comments: true
imagefeature:
tags: [work]
category: work
description: "密钥生成&Jetty配置"
---

使用mvn jetty:run同时启用http&https

<!--more-->

##密钥生成

使用如下命令生成https需要的密钥
{% highlight bash lineno  %}	
	keytool -genkey -alias keystore -keyalg RSA -validity 20000 -keystore keystore
	
{%  endhighlight %}

##配置修改

修改`Maven`配置：

{% highlight xml %}

<plugin> 
  <groupId>org.mortbay.jetty</groupId>  
  <artifactId>jetty-maven-plugin</artifactId>  
  <version>8.1.10.v20130312</version>  
  <configuration> 
    <scanIntervalSeconds>10</scanIntervalSeconds>  
    <reload>automatic</reload>  
    <webAppSourceDirectory>src/main/webapp</webAppSourceDirectory>  
    <webAppConfig> 
      <contextPath>/</contextPath> 
    </webAppConfig>  
    <connectors> 
      <connector implementation="org.eclipse.jetty.server.nio.SelectChannelConnector"> 
        <port>8080</port>  
        <maxIdleTime>30000</maxIdleTime> 
      </connector>  
      <connector implementation="org.eclipse.jetty.server.ssl.SslSelectChannelConnector"> 
        <port>8443</port>  
        <maxIdleTime>30000</maxIdleTime>  
        <keystore>keystore</keystore>  
        <password>123456</password>  
        <keyPassword>123456</keyPassword> 
      </connector> 
    </connectors>  
    <systemProperties> 
      <systemProperty> 
        <name>org.mortbay.util.URI.charset</name>  
        <value>UTF-8</value> 
      </systemProperty> 
    </systemProperties> 
  </configuration> 
</plugin>


{%  endhighlight %}

同时配置了http,https两种连接方式，`keystore`中的路径指向密钥的存储路径，下面两个密码是生成密钥的时候输入的。

##静态文件被锁点
使用mvn jetty:run启动项目后，会出现静态文件被锁定无法更改的问题，只需要把配置中的连接改为bio方式，如下：
{% highlight xml %}
<connector implementation="org.eclipse.jetty.server.bio.SocketConnector"/>
<connector implementation="org.eclipse.jetty.server.ssl.SslSocketConnector"/>
{%  endhighlight %}

