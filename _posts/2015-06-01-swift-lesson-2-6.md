---
layout: post
title: Swift教程第二章(6)
share: true
comments: true
imagefeature:
tags: [swift,work]
category: swift,work
description: "swift language part-2.6"
---

Swift教程第二章(6)--Swift基础

扩展、协议

<!--more-->
##扩展
**注意：如果定义一个扩展，此扩展对该类型的所有已有实例都是可用的**

使用关键字`extension`

###扩展属性
{% highlight swift %}
extension Double{
	var km:Double{return self*1000}
}
{%  endhighlight %}

###扩展构造器
{% highlight swift %}
extension Rect{
	init(...){

	}
}

{%  endhighlight %}

###扩展方法

{% highlight swift %}
extension Int{
	func repetitions(task:()->()){
		for i in 0..self{
			task()
		}
	}
}

{%  endhighlight %}


###修改实例方法
使用关键字`mutating`
{% highlight swift %}
extension Int{
	mutating func square(){
		self=self*self
	}
}

var someint=3
someint.square()
//someint is 9 now
{%  endhighlight %}

###下标（使用内部脚本实现）

向Int添加一个下标，返回十进制数字从又想做的第n个数字


{% highlight swift %}
extension Int{
	subscriot(digitIndex:Int)->Int{
		var decimalBase=1
		for _ in 1...digitIndex{
			decimalBase*=10
		}
		return (self/decimalBase)%10
	}
}
//123456789[0]=9
//123456789[1]=8
{%  endhighlight %}

##协议
使用关键字`protocol`用于统一方法和属性的名称，而不是现任和功能。协议能够被类、枚举、结构体实现，满足协议要求的类、枚举、结构体被称为协议的遵循着

很像其他语言的接口:)

语法：

{% highlight swift %}
protocol SomeProtocol{
	//协议内容
}

{%  endhighlight %}

###属性要求
{% highlight swift %}
//协议定义
protocol SomeProtocol{
	//只读属性
	var name:String{get}
	var age:Int{get set}
}
{%  endhighlight %}
用类实现协议时，使用`class`关键字来表示属性为类成员，用结构体或枚举实现协议时则使用`static`关键字来表示
{% highlight swift %}
protocol AnotherProtocol{
	class var someTypeProperty:Int{get set}
}
{%  endhighlight %}


###方法要求
{% highlight swift %}
protocol SomeProtocol{
	class func someTypeMethod()
}

protocol RandomNumberGenerator{
	func random()->Double
}
{%  endhighlight %}

###突变方法要求
能在方法或函数内部改变实力类型的方法成为突变方法，使用关键字`mutating`

**注意：用class实现协议中的mutating方法时，不用谢mutating关键字，用结构体、枚举实现协议中的mutating方法是必须有mutating关键字**

{% highlight swift %}
protocol Togglable{
	mutating func toggle()
}

enum OnOffswitch:Togglable{
	case Off,On
	mutating func toggle(){
		switch self{
			case Off:
				self=On
			case On:
				self=Off
		}
	}
}

var lightSwitch=OnOffswitch.Off
lightSwitch.toggle()
//lightSwitch现在为.On
{%  endhighlight %}

###协议类型

协议本身不识闲任何功能，但可以将它当做类型来使用，场景如下：

1. 作为函数，方法或构造其中的参数类型，返回值类型
2. 作为常熟，变量、属性的类型
3. 作为数组，字典或其他容器中的元素类型

###委托（代理）模式
允许类或结构体将一些需要他们负责的功能委托给其他类型。定义协议来封装那些需要被委托的和你熟和方法，使其遵循者拥有这些被委托的函数和方法。委托模式可以用来响应特定的动作或接受外部数据员提供的数据，而不需要知道外部数据源的类型。


###通过延展补充协议声明
当一个类型已经实现了协议中的所有要求，却没有声明时，使用`extension`关键字声明补充协议声明

{% highlight swift %}
extension hamster:SomeProtocol{}
{%  endhighlight %}
**把hamster当做SomeProtocol类型使用时还需要显示的类型转换**

###协议的继承
一个协议可以继承其他的协议

###协议合成
一个协议可由多个协议采用protocol<SomeProtocol,AnotherProtocol>这样的格式进行组合，成为协议组合(protocol composition)

{% highlight swift %}


{%  endhighlight %}


![][1]
**注意：协议合成并不会生成一个新协议类型，而是将多个协议合成一个临时的协议，超出范围后立刻失效。**

###可选协议类型
使用关键字`@optional`标记可选择实现的属性或方法，在协议前使用`@objc`


[1]:{{ site.url }}/assets/posts/2015-06/s-l-2-6-1.png "s-l-2-6-1 "



























