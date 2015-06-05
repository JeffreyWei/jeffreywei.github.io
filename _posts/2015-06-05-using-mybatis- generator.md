---
layout: post
title: MyBatis Generator使用
share: true
comments: true
imagefeature:
tags: [java,mybatis]
category: java,mybatis
description: "Using MyBatis Generator"
---

使用MyBatis Generator生成基础代码。

<!--more-->

##添加配置

###maven配置

在`pom.xml`的`plugins`中添加

{% highlight xml %}
<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.2</version>
    <configuration>
        <configurationFile>src/main/resources/generatorConfig.xml</configurationFile>
        <overwrite>true</overwrite>
        <verbose>true</verbose>
    </configuration>
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.34</version>
        </dependency>
        <dependency>
            <groupId>com.github.abel533</groupId>
            <artifactId>mapper</artifactId>
            <version>3.0.0</version>
        </dependency>
    </dependencies>
</plugin>


{%  endhighlight %}

此处使用了网友开源的组件生成定制的`Mapper`，并且为实体类添加了中文注释

###MyBatis Generator配置

`generatorConfig.xml`配置如下

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!-- 如果pom的plugin中没有配置使用的驱动，这里要使用绝对路径指定驱动 -->
    <!--<classPathEntry location="mysql-connector-java-5.1.34.jar"/>-->
    <context id="Mysql" targetRuntime="MyBatis3Simple" defaultModelType="flat">
    <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>
        <property name="javaFileEncoding" value="UTF-8"/>
        <plugin type="com.github.abel533.generator.MapperPlugin">
            <property name="mappers" value="com.github.abel533.mapper.Mapper"/>
        </plugin>

        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://127.0.0.1:3306/db?useUnicode=true&amp;characterEncoding=UTF-8"
                        userId=""
                        password="">
        </jdbcConnection>

        <javaModelGenerator targetPackage="com.aa.bb.cc.model" targetProject="/src/main/java/"/>

        <sqlMapGenerator targetPackage="xml" targetProject="/mybatis"/>

        <javaClientGenerator type="XMLMAPPER" targetPackage="com.aa.bb.cc.dao" targetProject="/src/main/java/"/>

        <!--匹配所有表-->
        <!--<table tableName="%">-->
        <!--<generatedKey column="id" sqlStatement="Mysql"/>-->
        <!--</table>-->

        <!-- 表对应实体类重命名 -->
        <table tableName="t_bill_info" domainObjectName="BillInfo"/>
    </context>
</generatorConfiguration>
{%  endhighlight %}

##生成文件

使用命令`mvn mybatis-generator:generate`生成文件

注：如果需要自定义生成的类注释，需要继承MyBatis Generator的`org.mybatis.generator.api.CommentGenerator`，默认使用的是`org.mybatis.generator.internal.DefaultCommentGenerator`，并使用maven发布后才能使用。

mvn mybatis-generator:generate