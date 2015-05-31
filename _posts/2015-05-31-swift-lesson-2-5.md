---
layout: post
title: Swift教程第二章(5)
share: true
comments: true
imagefeature:
tags: [swift,work]
category: swift,work
description: "swift language part-2.5"
---

Swift教程第二章(5)--Swift基础

构造过程、析构函数、自动引用计数、类型转换、类型嵌套

<!--more-->
##构造过程

###构造器

{% highlight swift %}
class SomeClass{
	var count:Double
	init(){
		count=0
	}
}

{%  endhighlight %}


###默认属性
{% highlight swift %}
class SomeClass{
	var count:Double=0
	init(){

	}
}

{%  endhighlight %}


###有参的构造函数
{% highlight swift %}
class SomeClass{
	var count:Double
	init(count:Double){
		self.count=count
	}
}

{%  endhighlight %}

###构造函数重载
定义多个init方法，包含的函数签名不同

###构造器链
Swift采用以下规则来限制构造器件的代理调用：

* 指定构造器必须总是向上代理
* 便利构造器总是横向代理

{% highlight swift %}
class Food{
	var name:String
	init(name:String){
		self.name=name
	}
	convenience init(){
		self.init(name:"[Unnamed]")
	}
}

{%  endhighlight %}

##析构函数
使用`deinit`标示析构函数

##自动引用计数
**使用ARC实现，引用计数只应用在类的实例，结构和美剧类型是值类型，并非引用类型，不是引用的方式来存储和传递的**

##类型转换

###类型检查
使用`is`进行类型检查

{% highlight swift %}
if _cla is SomeClass{
	//to do something
}
{%  endhighlight %}



###向下转型
使用`as`尝试向下转到他的子类型

{% highlight swift %}
if let _cla as? SomeClass{
	//use _cla to do something
}

{%  endhighlight %}

**注：转换并没有改变实例或值，只是把它当成某种类型使用**

###AnyObject类型
`AnyObject`可以代理任何class类型的实例


使用API的时候会使用到接受AnyObject[]类型的数组，它接受任意类型对象组成的数组

###Any类型
`Any`可以表示出了方法类型以外的任何类型

使用Any类型来混合不同的类型，包括非class类型


{% highlight swift %}
var tings=Any[]()
things.append(0)
things.append(0.0)
things.append("123")
things.append((1,2))
things.append(SomeClass())

{%  endhighlight %}

##类型嵌套
###定义
![][1]
![][2]
![][3]

###类型嵌套的引用
![][4]



[1]:{{ site.url }}/assets/posts/2015-05/s-l-2-5-1.png "s-l-2-5-1"
[2]:{{ site.url }}/assets/posts/2015-05/s-l-2-5-2.png "s-l-2-5-2"
[3]:{{ site.url }}/assets/posts/2015-05/s-l-2-5-3.png "s-l-2-5-3"
[4]:{{ site.url }}/assets/posts/2015-05/s-l-2-5-4.png "s-l-2-5-4"

