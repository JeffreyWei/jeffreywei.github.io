---
layout: post
title: matplotlib安装异常
share: true
comments: true
imagefeature:
tags: [python]
category: python
description: ""
---

记一次matplotlib升级遇到的问题

<!--more-->
    

升级matplotlib到最近版本



	sudo pip install -U matplotlib
	



尝试执行之前的代码提示`Matplotlib is building the font cache using fc-list. This may take a moment.`提示中的`fc-list`是linux中的命令，在mac上只需要打开字体库即可刷新字体缓存。刷新缓存后得到了第二个错误：`from six.moves import _thread,ImportError: cannot import name _thread`这类问题一般是包依赖造成的，在python命令行中执行：



	import six
	print six.__file__



发现使用的文件不是pip下的包而是python2.7自带的扩展包，按上面提示的路径删除对应的py文件即可。



    
    
    

