---
layout: post
title: 使用jsonp实现js跨域请求
share: true
comments: true
imagefeature:
tags: [javascript]
category: javascript
description: "ajax cross domain with jsonp"
---

页面内跨域请求

<!--more-->
##跨域限制
浏览器出于安全限制了不同域间的请求，jsonp使用动态生成前端js代码的方式绕过这个限制可以实现跨域请求。

##前端前端使用jquery封装的ajax{% highlight html %}
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<script type="text/javascript" src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
	<title></title>
</head>
<body>
<div id="content"></div>
<script>
	$(function () {
		$.getJSON("http://127.0.0.1:8080/ajax?callback=?",function(data){
			$("#content").html("This is "+data.name);
		});
	})
</script>
</body>
</html>{% endhighlight %}

调用时可以指定`callback`或由jquery自动生成callback的参数。##后端
后端代码使用springmvc实现一个可以处理异步请求的方法
{% highlight java %}
@RequestMapping(value = "/ajax") public @ResponseBody String  getAjaxResult(@RequestParam(value = "callback") String callback) {
	String json = "{'name':'张三'}";
	return callback + "(" + json + ")";
}{% endhighlight %}

后端服务需要可以处理`callback`参数，并在返回前端需要的回调函数。