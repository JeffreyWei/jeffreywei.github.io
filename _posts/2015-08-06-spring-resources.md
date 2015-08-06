---
layout: post
title: web项目中资源文件编译后改变 
share: true
comments: true
imagefeature:
tags: [java]
category: java
description: "using Apache Maven Resources Plugin"
---

解决resources目录中配置文件改变的问题，spring中使用静态注入

<!--more-->

##保持资源文件在编译后不变

由于项目中用到了17mon的IP库，数据已文件的形式放在项目下的resources，但每次编译完发现在target下省城的对应的文件已改变，猜测是使用maven编译时，对不同类型的资源文件进行了处理，尝试使用maven的`Maven Resources Plugin`插件排除特殊的资源文件

修改Maven Resources Plugin插件配置，排除指定类型的文件

{% highlight xml %}
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-resources-plugin</artifactId>
    <version>2.7</version>
    <configuration>
        <nonFilteredFileExtensions>
            <nonFilteredFileExtension>dat</nonFilteredFileExtension>
        </nonFilteredFileExtensions>
    </configuration>
</plugin>
{%  endhighlight %}

重新编译后，使用`md5`命令对比两个文件，target下的资源文件未改变，一起正常。


##Spring中的静态注入

要在spring中使用静态注入需要满足两个条件：

1. 声明静态属性的set方法
2. 使用`org.springframework.beans.factory.config.MethodInvokingFactoryBean`向类中的方法注入

如要注入path：

{% highlight java %}
private static String path;
public static setPath (newPath){
	path=newPath;
}
{%  endhighlight %}

spring配置如下：

{% highlight xml %}
<bean class="org.springframework.beans.factory.config.MethodInvokingFactoryBean">
    <property name="staticMethod" value="com.wei.shimao.utils.IPLoactionUtils.setPath"/>
    <property name="arguments">
        <value>
            ${ipdbpath}
        </value>
    </property>
</bean>
{%  endhighlight %}
