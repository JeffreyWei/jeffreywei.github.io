---
layout: post
title: Swift教程第一章
share: true
comments: true
imagefeature:
tags: [swift,work]
category: swift,work
description: "swift language part-1"
---

Swift教程第一章--认识Swift

<!--more-->


##Swift初见
**语句后不需要分号**

* 变量声明

	let 声明常量 
	var 生命变量
	//直接复制
	var a = 70
	//指定类型
	var b : Double =70

	var age=20;
	var msg="I am "
	var say=msg+String(age)
	or
	var say="I am \(age)"

* 数组&字典

	var people = ["Jack","Mike"]
	var rank =["Jack":1,"Mike":2]
	初始化
	var people=[] or var people =String[]()
	var rank=[:] or var Dictionary<String,Float>()

* 控制流

	for in 
	if boolExpression {}
	switch
	不需要break
	试一下没有default
	
	遍历一个字典
	for (key,value) in dic

	while boolExpression {}

	do{} while boolExpression

	普通循环
	for var i=0;i<3;i++{}
	or
	for i in 0..3{}
	*方式和python很像*

* 函数和闭包

	func greet(name:String,day:String)->String{
		return "Hello \(name), today is \(day).";
	}
	greet(" Bob", "Tue sday ")
	`->`指定返回值
	返回一个元组
	func getGasPrices() -> (Double, Double, Double) {
		freturn (3.59, 3.69, 3.79)
	}
	可变参数
	func su mOf(n umber s: I nt... ) -> Int {
		return 0
	}

	函数传递

	func ha sAnyM atche s(li st: I nt[], cond ition : Int -> B ool) -> Bool{}
	func lessThanTen(number: Int) -> Bool {}
	var numbers = [20, 19, 7, 12]
	hasAnyMatches(numbers, lessThanTen)

	numbers .map({
		(number: Int) -> Int in
		return result = 3 * number
	})


* 对象&类
	init 声明构造方法
	deinit	声明析构方法
	子类，父类使用`:`
	方法重写需要 function前添加`override`
	属性的get/set
		var perimeter: Double {
			get{1.0}
			set{perimeter = newValue}
		}
 	?的用法
 	let sideLength = optionalSquare ?.sid eLength
 	?前的变量如果为空后面的语句不会被执行
 

* 枚举

* 结构
	结构体是传值,类是传引用


* 接口&扩展
	接口 protocol
	mutating??
	
	扩展 extension

* 泛型
	<classType> <class : parentClassType>
	




