---
layout: post
title: maven使用总结
share: true
comments: true
imagefeature:
tags: [java,tool]
category: java,tool
description: "开发过程中maven总结"
---



<!--more-->

## module打包命名

1. 使用mvn package


```xml
    <packaging>war</packaging>
    <build>
        <finalName>uther</finalName>
    </build>        
```

2. mvn war:war

```xml

<plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-war-plugin</artifactId>
         <version>2.4</version>
         <configuration>
                <warName>uther</warName>
         </configuration>
</plugin>

```
3. mav jar:jar

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>2.3.2</version>
    <configuration>
        <finalName> uther </finalName>                   
    </configuration>
</plugin> 

```

## 跳过单元测试


1. `-Dmaven.test.skip=true`不执行、不编译也单元测试
2. `-DskipTests`不执行，会编译待援测试

对应上面两种方式的pom文件配置

* 不执行、不编译

```xml   
<plugin>    
    <groupId>org.apache.maven.plugins</groupId>    
    <artifactId>maven-surefire-plugin</artifactId>    
    <version>2.5</version>    
    <configuration>    
        <skip>true</skip>    
    </configuration>    
</plugin>

```

* 不执行、仅编译

```xml
<plugin>    
    <groupId>org.apache.maven.plugins</groupId>    
    <artifactId>maven-surefire-plugin</artifactId>    
    <version>2.5</version>    
    <configuration>    
        <skipTests>true</skipTests>    
    </configuration>    
</plugin>

```

## 多模块同时编译

项目中模块一般都是有依赖关系的，打包一个项目的同时往往需要先打包一些依赖模块，在项目根目录执行`mvn clean package -pl module_name -am`

参数解释：
<pre>
-am --also-make 同时构建所列模块的依赖模块；If project list is specified, also build projects required by the list
-amd -also-make-dependents 同时构建依赖于所列模块的模块； If project list is specified, also build projects that depend on projects on the list
-pl --projects <arg> 构建制定的模块，模块间用逗号分隔；Comma-delimited list of specified reactor projects to build instead of all projects. A project can be specified by [groupId]:artifactId or by its relative path.
-rf -resume-from <arg> 从指定的模块恢复反应堆。Resume reactor from specified project.
</pre>


