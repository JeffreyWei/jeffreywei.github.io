---
layout: post
title: java注解回顾
share: true
comments: true
imagefeature:
tags: [java,annotation]
category: java,annotation
description: "java annotation"
---

上次提到了lombok,顺便回顾一下注解

<!--more-->

## 元注解

### @Retention

注解的保留期，分为三种

* RetentionPolicy.SOURCE 源码期注解
* RetentionPolicy.CLASS 编译期注解
* RetentionPolicy.RUNTIME 运行期注解

### @Documented

注解的相关属性会包含到javadoc中

### @Target

注解的作用范围，有以下六种

* ElementType.ANNOTATION_TYPE 任意类型
* ElementType.CONSTRUCTOR 构造方法
* ElementType.FIELD 属性
* ElementType.LOCAL_VARIABLE 局部变量
* ElementType.METHOD 类方法
* ElementType.PACKAGE 包
* ElementType.PARAMETER 方法参数
* ElementType.TYPE 类型：类、接口、枚举

### @Inherited

子类在无显示标注注解的情况下，可以继承父类标注的注解。

```java

@Inherited
@Retention(RetentionPolicy.RUNTIME)
public @interface Tag{
}

@Tag
public class A {

}

public class B extern A{

}

```

B无显示声明注解，继承父类A的注解@Tag

### @Repeatable

JDK 8中引入,注解是都可重复标注

```java
public @interface interface Tag{
  Job[] value()
}

@Repeatable(Tag.class)
public @interface interface Job{
	String jobName default "";
}

@Job("doctor")
@Job("teacher")
pubic class Man{

}

```

### 注解的应用

通过反射可以获得类的注解及注解属性,为类提供扩展。