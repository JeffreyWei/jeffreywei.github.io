---
layout: post
title: Swift教程第二章(4)
share: true
comments: true
imagefeature:
tags: [swift,work]
category: swift,work
description: "swift language part-2.4"
---

Swift教程第二章(4)--Swift基础

方法、继承
<!--more-->
##方法

###实例方法

{% highlight swift %}
class Counter{
	var count=0
	func increment{
		count++
	}
	func incrementBy(amount:Int){
		count+=amount
	}
	func reset(){
		count=0
	}
	func foo(#foo:Int){
		count+=foo
	}
	//or
	//func foo(foo _foo:Int){
	//	count+=_foo
	//}
}

{%  endhighlight %}
`#`为外部参数名称

###self
`self`为该实例本身

###类型方法（type methods）
声明累的类型方法，在方法的func前加上class;声明结构体、枚举的类型方法在func前加上static

class:
{% highlight swift %}
class SomeClass{
	class func someTypeMethod(){
		//to do something
	}
}
SomeClass.someTypeMethod()
{%  endhighlight %}

struct:
{% highlight swift %}
struct SomeClass{
	static func someTypeMethod(){
		//to do something
	}
}
SomeClass.someTypeMethod()
{%  endhighlight %}

##附属脚本

使用关键字`subscript`，语法：

{% highlight swift %}
subscript(index:Int)->Int{
	get{
		//返回
	}
	set(newValue){
		//赋值
	}
}

{%  endhighlight %}

🌰:)
{% highlight swift %}
struct TimeTable{
	let mutiplier:Int
	//只读型
	subscript(index:Int)->Int{
		return mutiplier*index
	}
}
let threeTimesTable=TimeTable(mutiplier:3)
println("\(threeTimesTable[6])")
//输出  18
{%  endhighlight %}

##继承

###父类

{% highlight swift %}
class Vehicle{
	var numberOfWheels:Int
	var maxPassengers:Int
	func description()->String{
		return "\(numberOfWheels)\(maxPassengers)"
	}
	//无参构造方法
	init(){
		numberOfWheels=0
		maxPassengers=1
	}
}

let someBehicle =Vehicle()
{%  endhighlight %}
使用`init`关键字声明类的构造方法

###子类生成

{% highlight swift %}
class Bicycle:Vehicle{
	init(){
		//使用父类的构造方法
		super.init()
		numberOfWheels=2
	}
}

{%  endhighlight %}

###重写(Overriding)
子类可以继承实例方法，类方法，属性，附属脚本或提供自己对以上类型的定制的实现使用关键字`override`

{% highlight swift %}
class Car:Vehicle{
	var speed:Double=0.0
	init{
		super.init()
		maxPassengers=5
		numberOfWheels=4
	}
	override func description()->String{
		return "\(maxPassengers)\(numberOfWheels)"
	}
}

{%  endhighlight %}

###防止重写
使用`@final`关键字，例如:

* @final var
* @final func
* @final class func
* @final subscript

声明无法被集成的类使用`@final class`


