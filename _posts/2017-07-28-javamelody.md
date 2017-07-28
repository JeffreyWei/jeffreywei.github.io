---
layout: post
title: java进程监控-javamelody
share: true
comments: true
imagefeature:
tags: [java,jvm]
category: java,jvm
description: "监控工具javamelody"
---



<!--more-->

本文介绍一种集成在项目中的java进程监控工具[JavaMelody](https://github.com/javamelody/javamelody)


## 集成在web项目中使用

### 依赖
```xml

<!-- javamelody-core -->
 <dependency>
     <groupId>net.bull.javamelody</groupId>
     <artifactId>javamelody-core</artifactId>
     <version>1.68.1</version>
 </dependency>
 <!-- itext, option to add PDF export -->
 <dependency>
     <groupId>com.lowagie</groupId>
     <artifactId>itext</artifactId>
     <version>2.1.7</version>
     <exclusions>
         <exclusion>
             <artifactId>bcmail-jdk14</artifactId>
             <groupId>bouncycastle</groupId>
         </exclusion>
         <exclusion>
             <artifactId>bcprov-jdk14</artifactId>
             <groupId>bouncycastle</groupId>
         </exclusion>
         <exclusion>
             <artifactId>bctsp-jdk14</artifactId>
             <groupId>bouncycastle</groupId>
         </exclusion>
     </exclusions>
 </dependency>
 <!--xml json expoer-->
 <dependency>
     <groupId>com.thoughtworks.xstream</groupId>
     <artifactId>xstream</artifactId>
     <version>1.4.2</version>
 </dependency>
 <dependency>
     <groupId>org.jrobin</groupId>
     <artifactId>jrobin</artifactId>
     <version>1.5.9</version>
 </dependency>

```

### 访问账户设置

设置一个访问密码

![][1]


> -Djavamelody.authorized-users=root:root


### 效果

![][2]
![][3]
![][4]

[1]: {{ site.url }}/images/posts/2017-07/javamelody_auth_condif.png "javamelody_auth_condif"
[1]: {{ site.url }}/images/posts/2017-07/javamelody_web_1.png "javamelody_web_1"
[1]: {{ site.url }}/images/posts/2017-07/javamelody_web_2.png "javamelody_web_2"
[1]: {{ site.url }}/images/posts/2017-07/javamelody_web_3.png "javamelody_web_3"
