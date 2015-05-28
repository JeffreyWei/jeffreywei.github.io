---
layout: post
title: Swift教程第二章(2)
share: true
comments: true
imagefeature:
tags: [swift,work]
category: swift,work
description: "swift language part-2.2"
---

Swift教程第二章(2)--Swift基础

* 闭包
* 枚举
* 类和结构体

<!--more-->
	
##闭包

* 闭包表达式
{% highlight swift %}
	
func backwards (s1:String,s2:String)->Bool{
	return s1>s2
}
var reversed=sort(names,backwards)

//闭包表达式语法
	
//格式	
	
{（parameters）-> returnType in 
	//statements
}		
	
//上面的方法改写形式
	
var reversed=sort(names,{(s1:String,s2:String)->Bool in 
	return a1>s2
})

//根据上下文判断类型

var reversed=sort(names,{s1,s2 in return a1>s2})
	
//单行表达式闭包可以省略return

var reversed = sort(names,{s1,s2 in s1 > s2})
	
//参数名简写

//通过$0,$1,$2等名字来引用闭包的参数值
	
var reversed = sort(names,{$0 > $1})

//运算符函数

var reversed = sort(names , >)
{%  endhighlight %}
		
* Trailing闭包

		var reversed = sort(names){$0 > $1}
		
* 捕获（Caputure）
{% highlight swift %}

func makeIncrementor(forIncrement amount:Int) -> ()->Int{
	var total=0;
	func incrementor()->Int{
		total+=amount
		return total
	}
	return incrementor
}	
{%  endhighlight %}

##枚举

* 枚举定义
{% highlight swift %}
	
enum CompassPoint {
	case North
	case South
	case East
	case West
}

//or
		
enum Planet{
	case Mercury,Benus,Earth,Mars,Jupiter,Saturn,Uranus,Nepturn
}
{%  endhighlight %}

* 使用

{% highlight swift %}

var directionToHead = CompassPoint.West
//change value
directionToHead = .East
{%  endhighlight %}
		
* 关联值
{% highlight swift %}

enum Barcode{
	case UPCA(Int,Int,Int)
	case QRCode(String)
}

var productBarcode = Barcode.UPCA(8,634423,3)
{%  endhighlight %}
		
* 原始值
{% highlight swift %}

enum ASCIIControlCharacter : Character{
	case Tab = "\t"
	case LineFeed = "\n"
	case CarriageReturn = "\r"
}		

enum Planet : Int{
	case Mercury:1,Benus,Earth,Mars,Jupiter,Saturn,Uranus,Nepturn
}
//`Benus`的值将为2
	
let earthsOrder = Planet.Earth.toRaw()
//earthsOrder is 3

let possiblePlanet = Planet.fromRaw(7)
//possiblePlanet is of type Planet and equals Planet.Uranus
{%  endhighlight %}
		
##类和结构体

