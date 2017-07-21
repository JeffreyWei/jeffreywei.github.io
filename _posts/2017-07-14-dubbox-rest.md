---
layout: post
title: dubbo升级，集成rest服务
share: true
comments: true
imagefeature:
tags: [java,rpc]
category: java,rpc
description: "项目升级dubbo版本，集成多协议，支持rest"
---



<!--more-->

## 修改项目依赖

升级dubbo->dubbox,添加rest协议依赖，修改pom.xml

```xml
 <dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>dubbo</artifactId>
    <version>2.8.4</version>
</dependency>
<dependency>
    <groupId>org.jboss.resteasy</groupId>
    <artifactId>resteasy-client</artifactId>
    <version>3.0.7.Final</version>
</dependency>
<dependency>
    <groupId>org.jboss.resteasy</groupId>
    <artifactId>resteasy-netty</artifactId>
    <version>3.0.7.Final</version>
</dependency>
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
    <version>1.0.0.GA</version>
</dependency>

```

## 修改dubbo-provider配置

```xml
<dubbo:protocol name="dubbo" port="${dubbo.port}" threads="1000"/>
<dubbo:protocol path="rest" name="rest" port="8077" server="netty" threads="1000" />
<dubbo:service interface="com.yizhenmoney.damocles.uther.rpc.Rcm4TJYAdapter" ref="tjyService" protocol="dubbo,rest"   registry="provideregistry"  timeout="500"/>
```

上面列出来的是主要的修改部分，定义了两种协议方式，rest中的`path`为服务的根路径，`port`为rest对外的端口，'server'为处理处理请求的方式，此处用的是netty

## 实现方法修改

```java

@Service
@Path("tjy")
public class TjyService implements Rcm4TJYAdapter {
@Override
	@POST
	@Path("rcmRoute") @Consumes({"application/json;charset=UTF-8"})@Produces("application/json;charset=utf-8")
	public String rcmRoute(String method, String sign, String biz_data) {
		log.info("method:" + method + "biz_date:" + biz_data);
//验签
		String result = ERROR;
		Boolean isok = false;
		try {
			isok = true;//RsaEncrypt.getINSTANCE().doCheck(biz_data, Base64Utils.decode(sign));
		} catch (Exception e) {
			log.error("验签失败", e);
		}
		if (!isok) {
			return result;
		}
		try {
			switch (method) {
				case "saveAppend_info":
					//补充信息保存
					result = saveAppend_info(biz_data);
					break;
				//运营商报告
				case "push_mobile_report":
					result = save_mobile_report(biz_data);
					break;
			}
		} catch (Exception e) {
			log.error("淘金云业务处理失败,method:" + method + ",biz_data:" + biz_data, e);
		}
		log.info("method:" + method + ",biz_date:" + biz_data + ",result:" + result);
		return result;
	}
}


```
需要修改的有两处，一个在类上定义类的访问路径，一个在方法上定义方法的路径，`@Consumes`、`@Produces`用于设置请求及返回值的具体格式。

## 测试

* postman
* IDEA-RESR Client

设置头部为`Content-Type:application/json`，拼接请求参数发送请求。


## 备注

dubbox中使用的[resteasy](http://resteasy.jboss.org/)实现的rest协议，据官网介绍使用默认协议与rest协议性能比1：1.5 ，以下特点引用自[dubbox-rest](http://dangdangdotcom.github.io/dubbox/rest.html)介绍

### REST的优点

以下摘自维基百科：

* 可更高效利用缓存来提高响应速度
* 通讯本身的无状态性可以让不同的服务器的处理一系列请求中的不同请求，提高服务器的扩展性
* 浏览器即可作为客户端，简化软件需求
* 相对于其他叠加在HTTP协议之上的机制，REST的软件依赖性更小
* 不需要额外的资源发现机制
* 在软件技术演进中的长期的兼容性更好


这里我还想特别补充REST的显著优点：基于简单的文本格式消息和通用的HTTP协议，使它具备极广的适用性，几乎所有语言和平台都对它提供支持，同时其学习和使用的门槛也较低。
### 应用场景
正是由于REST在适用性方面的优点，所以在dubbo中支持REST，可以为当今多数主流的远程调用场景都带来（显著）好处：

1. 显著简化企业内部的异构系统之间的（跨语言）调用。此处主要针对这种场景：dubbo的系统做服务提供端，其他语言的系统（也包括某些不基于dubbo的java系统）做服务消费端，两者通过HTTP和文本消息进行通信。即使相比Thrift、ProtoBuf等二进制跨语言调用方案，REST也有自己独特的优势（详见后面讨论）

2. 显著简化对外Open API（开放平台）的开发。既可以用dubbo来开发专门的Open API应用，也可以将原内部使用的dubbo service直接“透明”发布为对外的Open REST API（当然dubbo本身未来最好可以较透明的提供诸如权限控制、频次控制、计费等诸多功能）

3. 显著简化手机（平板）APP或者PC桌面客户端开发。类似于2，既可以用dubbo来开发专门针对无线或者桌面的服务器端，也可以将原内部使用的dubbo service直接”透明“的暴露给手机APP或桌面程序。当然在有些项目中，手机或桌面程序也可以直接访问以上场景2中所述的Open API。

4. 显著简化浏览器AJAX应用的开发。类似于2，既可以用dubbo来开发专门的AJAX服务器端，也可以将原内部使用的dubbo service直接”透明“的暴露给浏览器中JavaScript。当然，很多AJAX应用更适合与web框架协同工作，所以直接访问dubbo service在很多web项目中未必是一种非常优雅的架构。

5. 为企业内部的dubbo系统之间（即服务提供端和消费端都是基于dubbo的系统）提供一种基于文本的、易读的远程调用方式。

6. 一定程度简化dubbo系统对其它异构系统的调用。可以用类似dubbo的简便方式“透明”的调用非dubbo系统提供的REST服务（不管服务提供端是在企业内部还是外部）

需要指出的是，我认为1～3是dubbo的REST调用最有价值的三种应用场景，并且我们为dubbo添加REST调用，其最主要到目的也是面向服务的提供端，即开发REST服务来提供给非dubbo的（异构）消费端。
