---
layout: post
title: springmvc中配置FreeMarker
share: true
comments: true
imagefeature:
tags: [java,FreeMarker,spring]
category: java,FreeMarker,spring
description: "springmvc with FreeMarker"
---

前端模板freemarker配置

<!--more-->
 		
##freemarker与JSP

* freemarker是一种模板技术，类似的技术在python与nodejs众比较常见，语法直观
* JSP会在编译器生成集成Servlet的class文件

除此之外freemarker也被用于代码生成、票据报表、邮件生成等方面

##springmvc中集成freemarker
pom.xml中添加freemarker依赖

        <dependency>
            <groupId>org.freemarker</groupId>
            <artifactId>freemarker</artifactId>
            <version>2.3.23</version>
        </dependency>


springmvc相关配置

{% highlight xml %}
<bean id="freemarkerConfigurer"
      class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">
    <property name="templateLoaderPath" value="/WEB-INF/views/" />
    <property name="defaultEncoding" value="utf-8" />
    <property name="freemarkerSettings">
        <props>
            <prop key="template_update_delay">10</prop>
            <prop key="locale">zh_CN</prop>
            <prop key="datetime_format">yyyy-MM-dd HH:mm:ss</prop>
            <prop key="date_format">yyyy-MM-dd</prop>
            <prop key="number_format">#.##</prop>
        </props>
    </property>
</bean>

<bean id="viewResolver" class="org.springframework.web.servlet.view.freemarker.FreeMarkerViewResolver">
    <property name="viewClass" value="org.springframework.web.servlet.view.freemarker.FreeMarkerView"></property>
    <property name="suffix" value=".jsp" />
    <property name="contentType" value="text/html;charset=utf-8" />
    <property name="exposeRequestAttributes" value="true" />
    <property name="exposeSessionAttributes" value="true" />
    <property name="exposeSpringMacroHelpers" value="true" />
</bean>
{%  endhighlight %}

##模板文件常用语法
{% highlight jsp %}
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">  
<html>  
<head>  
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">  
<title>Insert title here</title>  
</head>  
<body>  
  
描述：${description}  
<br/>  
集合大小:${nameList?size}  
<br/>  
迭代list集合：  
<br/>  
<#list nameList as names>  
这是第${names_index+1}个人，叫做：<label style="color:red">${names}</label>  
if判断：  
<br/>  
<#if (names=="张三")>  
 他是科长  
<#elseif (names=="李四")>       <#--注意这里没有返回而是在最后面-->   
 他是处长  
<#else>  
他是群众 
</#if>  
<br/>  
</#list>  
迭代map集合：  
<br/>  
<#list weaponMap?keys as key>  
key--->${key}<br/>  
value----->${weaponMap[key]!("null")}  
<#--   
fremarker 不支持null, 可以用！ 来代替为空的值。  
其实也可以给一个默认值    
value-----${weaponMap[key]?default("null")}  
还可以 在输出前判断是否为null  
<#if weaponMap[key]??></#if>都可以  
-->  
  
<br/>  
</#list>  
include导入文件：  
<br/>  
<#include "include.html"/>  
  
</body>  
</html>  
{%  endhighlight %}

模板代码中可以直接读取session中级response中填入的属性值
