---
layout: post
title: spring session
share: true
comments: true
imagefeature:
tags: [java,web]
category: java,web
description: "using spring session"
---

为实现分布式部署，实现session共享，即使在开发环境中经常重启服务已不用担心重新登录的问题。

<!--more-->

## 方案选择

方案一：在容器上做配置


方案二：在应用上做配置

考虑到上线后容器可能会更换，或版本不容带来的问题，采用第二种方案

## 配置

添加依赖

	     <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-redis</artifactId>
            <version>1.5.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.session</groupId>
            <artifactId>spring-session</artifactId>
            <version>1.2.0.RC2</version>
        </dependency>
        
在web.xml中添加filter并放在最外层。

	<filter>
        <filter-name>springSessionRepositoryFilter</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>springSessionRepositoryFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    
在spring配置中添加

	    <bean class="org.springframework.session.data.redis.config.annotation.web.http.RedisHttpSessionConfiguration"/>
    <bean id="jedisConnectionFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
        <property name="hostName" value="${redis.host}"/>
        <property name="timeout" value="${redis.timeout}"/>
    </bean>
    
启动项目，检查redis中session存储及过期时间。

    
    
    
    
    
    

