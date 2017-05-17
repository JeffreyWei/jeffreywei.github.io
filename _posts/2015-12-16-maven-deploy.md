---
layout: post
title: maven发布带有javadoc、source的库文件
share: true
comments: true
imagefeature:
tags: [java,maven]
category: java,maven
description: "using maven deploy with source & javadoc"
---
如果只发布打包的jar,在其他项目中引用看到的参数都是以var1...的形式存在，而且不包含注释文档，本文介绍如何向布带有源码、注释的jar.

<!--more-->

##maven配置
向maven配置文件`settings.xml`中添加`servers`段，例如：

{% highlight xml %}
    <server>
     <id>thirdparty</id>
     <username>admin</username>
     <password>admin</password>
    </server>
    <server>
     <id>snapshots</id>
     <username>admin</username>
     <password>admin</password>
  	</server>

{%  endhighlight %}

分别用于release、snapshot版本发布.

##project配置

修改项目下的`pom.xml`文件

{% highlight xml %}
<plugin>
    <artifactId>maven-source-plugin</artifactId>
    <executions>
        <execution>
            <id>attach-sources</id>
            <phase>deploy</phase>
        </execution>
    </executions>
</plugin>
<plugin>
    <artifactId>maven-javadoc-plugin</artifactId>
    <executions>
        <execution>
            <id>attach-javadocs</id>
            <phase>deploy</phase>
        </execution>
    </executions>
</plugin>
<plugin>
    <!-- explicitly define maven-deploy-plugin after other to force exec
        order -->
    <artifactId>maven-deploy-plugin</artifactId>
    <executions>
        <execution>
            <id>deploy</id>
            <phase>deploy</phase>
            <goals>
                <goal>deploy</goal>
            </goals>
        </execution>
    </executions>
</plugin>
{%  endhighlight %}

##发布
使用如下命令发布

		mvn source:jar javadoc:jar deploy

这样在其他项目引用时就可以获取到文档及源码。
