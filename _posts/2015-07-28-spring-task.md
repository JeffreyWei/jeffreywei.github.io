---
layout: post
title: spring-task使用注意事项
share: true
comments: true
imagefeature:
tags: [work]
category: work
description: "记录spring-task使用过程中遇到的问题"
---

在spring的主配置和spring-mvc配置中添加task。
<!--more-->


##使用主配置文件
task相关配置内容如下：
{% highlight xml  %}	
<context:component-scan base-package="com.yizhenmoney.damocles.crs.scheduler"/>
{%  endhighlight %}

业务逻辑代码

{% highlight java %}
@Component
@EnableScheduling
public class AgeScheduler {
    @Scheduled(cron = "0 0 0 1 1 ?")
    public void execute() {
	}
}
{%  endhighlight %}


**注：**

1. 需要在类上添加`@EnableScheduling`注解；
2. 如果spring的主配置中配置了`default-lazy-init="true"`属性，需要在task的类上添加`@Lazy(value=false)`使其启动时创建对象。



##使用mvc配置文件
配置添加如下内容：

{% highlight xml  %}	
xmlns:task="http://www.springframework.org/schema/task"
http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task-3.2.xsd
<context:component-scan base-package="com.yizhenmoney.damocles.crs.controller,com.yizhenmoney.damocles.crs.scheduler"/>
<task:annotation-driven/>
<task:executor id="myExecutor" pool-size="5"/>
<task:scheduler id="myScheduler" pool-size="10"/>
{%  endhighlight %}

业务逻辑代码

{% highlight java %}
@Component
public class AgeScheduler {
    @Scheduled(cron = "0 0 0 1 1 ?")
    public void execute() {
	}
}
{%  endhighlight %}{% highlight xml %}
<connector implementation="org.eclipse.jetty.server.bio.SocketConnector"/>
<connector implementation="org.eclipse.jetty.server.ssl.SslSocketConnector"/>
{%  endhighlight %}

