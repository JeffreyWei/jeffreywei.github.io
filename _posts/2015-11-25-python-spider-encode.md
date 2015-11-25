---
layout: post
title: 当爬虫遇到压缩页面&GBK编码
share: true
comments: true
imagefeature:
tags: [python, spider]
category: python, spider
description: "解决页面压缩&中文乱码问题"
---

目标站点使用gzip或者中文编码

<!--more-->

[网络VIP账号爬虫工具](https://github.com/JeffreyWei/xunleiaccount)执行时出错，首先使用浏览器访问检查页面样式是否变更，确认代码中样式规则仍然是适用的，后使用同样的方法访问站点首页正常，在访问账号页面时返回数据为乱码，使用`curl`查看页面同样返回乱码，查看浏览器请求，返回的头部确实有压缩标记，处理方法，需要在得到返回内容后使用对应的解压缩即可，这里给出Gzip的实现，如下：
{% highlight python %}

def getPageHTML(url):
	req = urllib2.Request(url);
	req.add_header('Accept-Encoding', 'gzip, deflate');
	f = urllib2.urlopen(req, timeout=30)
	html = f.read()
	# gzip解压缩
	if html[:6] == '\x1f\x8b\x08\x00\x00\x00':
		html = gzip.GzipFile(fileobj=cStringIO.StringIO(html)).read()
	html = html.decode('gbk')
	return html

{%  endhighlight %}