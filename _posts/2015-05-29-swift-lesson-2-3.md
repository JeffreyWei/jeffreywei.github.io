---
layout: post
title: Swift教程第二章(3)
share: true
comments: true
imagefeature:
tags: [swift,work]
category: swift,work
description: "swift language part-2.3"
---

Swift教程第二章(3)--Swift基础

类和结构体、指针、属性

<!--more-->
##类和结构体
###定义

{% highlight swift %}
//类
class SomeClass{
	//class definition goes here
}

//结构	
struct SomStructure{
	//structure definition goes here
}
{%  endhighlight %}
		
###类和结构体实例

{% highlight swift %}
let someClass=SomeClass()
let somStructure=SomStructure()

{%  endhighlight %}

###属性访问
使用`.`访问类或结构体中的属性

###结构体类型的构造器

{% highlight swift %}
struct Resolution{
	var width = 0
	var heigth = 0
}

var resolution=Resolution(width:300,heigth:400)
{%  endhighlight %}

**与结构体不同，类实例没有默认的构造器**

###结构体和枚举是值类型
**结构体、枚举总是通过被复制的方式在代码中传递**
{% highlight swift %}
let hd = Resolution(width:300,heigth:400)
var cinema = hd
cinema.width = 2048
println("cinema is now \(cinema.width) pixels wide")
// 输出 "cinema is now 2048 pixels wide"
println("hd is still \(hd.width ) pixels wide")
// 输出 "hd is still 1920 pixels wide"
{%  endhighlight %}
**`var cinema = hd`传递的时hd的拷贝，对cinema的修改不影响hd的属性，枚举也遵循此规则**

###类是引用类型
**引用类型在被富裕到一个变量，常量或者被传递到一个函数时，操作的是实例本身而不是其拷贝**

##恒等运算符

`===`(`!==`)表示连个类类型（class type）的常量或者变量引用同一个类实例
`==`(`!=`)表示两个实力的值“相等”或“相同”，判定时要遵照类设计者定义的评判标准

##指针
**Swift中使用其他语言指针时，与其他的变量或常量定一方是相同**


###集合（Collection）类型的复制和拷贝行为

Swift中数组（Array）和字典（DIctionary）类型均以结构体的形式实现

字典类型被复制的是对象的拷贝

数组类型被复制的时对象的引用，当改变数组结构时（添加或移除一个元素）会在原数组的拷贝对象上操作

数组对象上调用`unshare()`方法可以使此对象编程一个为一个拷贝

##属性

###属性的延迟初始化
使用`@lazy`表示一个延迟初始化的属性

{% highlight swift %}
class DataImporter{
	var	fileName="dataFile"
}

class DataManager{
	@lazy var importer=DataImporter()
	var data=String [] ()
}

let manager=DataManager()
manager.data+="some data"
manager.data+="some more data"
//此时importer对象并没有被创建

//在被使用时对象被创建
print(manager.importer.fileName)
//输出"dataFile"
{%  endhighlight %}

###属性的get/set

![][1]

**set中默认使用newValue代替新输入的对象**

![][2]

###只读属性

{% highlight swift %}
struct Cuboid{
	var volume:Double{
		return 1;
	} 
}
{%  endhighlight %}

###属性监视器
`willSet`在设置新值之前被调用(值默认表示为newValue)
`didSet`在设置新值之后被调用(值默认表示为oldValue)

{% highlight swift %}
class StepCounter{
	var totalSteops:Int=0
	willSet(){
		print(值默认表示为newValue)
	}
	didSet{
		if totalSteops>oldValue{
			print(oldValue)
		}
	}

}

{%  endhighlight %}

###静态属性
使用`static`标注的变量类型

{% highlight swift %}
class SomeClass{
	static var count:Int=0
}

{%  endhighlight %}



[1]: {{ site.url }}/assets/posts/2015-05/properties_0.png.png "properties_0"
[2]: {{ site.url }}/assets/posts/2015-05/properties_1.png.png "properties_1"

