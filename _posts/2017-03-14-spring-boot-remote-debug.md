---
layout: post
title: 远程调试spring-boot项目
share: true
comments: true
imagefeature:
tags: [java,spring-boot]
category: java
description: ""
---


<!--more-->

项目越来越多的使用spring-boot,远程调试的需求应运而生，实现方式有两种

## maven

{% highlight xml %}

<project>
  ...
  <build>
    ...
    <plugins>
      ...
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <version>1.1.12.RELEASE</version>
        <configuration>
          <jvmArguments>
            -Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=5005
          </jvmArguments>
        </configuration>
        ...
      </plugin>
      ...
    </plugins>
    ...
  </build>
  ...
</project>
{%  endhighlight %}

或

	mvn spring-boot:run -Drun.jvmArguments="-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=5005"
## shell

添加运行参数`-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005`
    
