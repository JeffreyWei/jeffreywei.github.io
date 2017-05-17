---
layout: post
title: web项目集成swagger
share: true
comments: true
imagefeature:
tags: [java,web]
category: java
description: ""
---



<!--more-->

## 添加依赖

	<dependency>
		<groupId>io.springfox</groupId>
		<artifactId>springfox-swagger2</artifactId>
		<version>2.2.2</version>
	</dependency>
	<dependency>
		<groupId>io.springfox</groupId>
		<artifactId>springfox-swagger-ui</artifactId>
		<version>2.2.2</version>
	</dependency>

## 添加spring配置

{% highlight xml %}

<bean class="com.xx.uther.utils.SwaggerConfig"/>
<mvc:resources location="classpath:/META-INF/resources/" mapping="swagger-ui.html"/>
<mvc:resources location="classpath:/META-INF/resources/webjars/" mapping="/webjars/**"/>

{%  endhighlight %}

构造的类用于配置swagger，内容如下

{% highlight java %}

@EnableWebMvc
@EnableSwagger2
@Configuration
public class SwaggerConfig extends WebMvcConfigurationSupport {

	@Bean
	public Docket createRestApi() {
		return new Docket(DocumentationType.SWAGGER_2)
				.apiInfo(apiInfo())
				.select()
				.apis(RequestHandlerSelectors.basePackage("com.xx.uther.web.controller.api"))
				.paths(PathSelectors.any())
				.build();
	}

	private ApiInfo apiInfo() {
		return new ApiInfoBuilder()
				.title("RCM APIs")
				.contact("RCM项目组")
				.version("V2.0.0")
				.build();
	}
}

{%  endhighlight %}

## 改造Controller层

通常的用法如下

{% highlight java %}

@RestController
@RequestMapping(value = "/api", method = RequestMethod.GET)
@Api(description = "xxxx服务")
public class ServiceImplController {
	@Autowired
	RcmAdapter rcmAdapterImp;
	@Autowired
	Rcm4webAdapter rcm4webAdapter;


	@ApiOperation(value = "网认证方式", notes = "互联网认证方式")
	@RequestMapping(value = "/getAuthenticationFromRong360ByH5", method = RequestMethod.GET)
	public Object getAuthenticationFromRong360ByH5(@RequestParam String typeEnum, @RequestParam String customerid) {
		return rcm4webAdapter.getAuthenticationFromRong360ByH5(typeEnum, customerid);
	}
}

{%  endhighlight %}

## 效果

访问`http://IP:port/{context-path}/swagger-ui.html`

![ ][1]
![ ][2]


[1]: {{ site.url }}/images/posts/2017-03/swagger_1.png "swagger_1"
[2]: {{ site.url }}/images/posts/2017-03/swagger_2.png "swagger_2"
