---
layout: post
title: Swift教程第二章
share: true
comments: true
imagefeature:
tags: [swift,work]
category: swift,work
description: "swift language part-2"
---

Swift教程第二章--Swift基础

<!--more-->
##Swift基础

* **基本类型**

	Int,Double,Float,Bool,String,Array,Dictonary

* **常量和变量的命名**
	
	允许使用`unicode`字符命名变量名可。
	
* **注释**

	`// 这是一个注释`

	or

	`/*这是`

	`多行注释*/`

* **分号**

	每条语句并非强制要求使用`;`如一行多条语句可以用作分隔，但不推荐。
	
	例如：
	
		let name="jack";pringln(name)

* **数值型字面量**

		十进制，没有前缀
		二进制,Ob
		八进制，O0
		十六进制，Ox

* **元组**

	let http200Status = ( statusCode : 200 , description: "OK")
	println("this status code is \(http200Status.statusCode)")

* **断言**

	条件成立继续执行，不成立则停止
	let age=-3
	assert(age>=0,"error")

##基本运算符

* **区间运算符**

	[1,3]	1...3
	
	[1,3)	1..3
		
##流程控制

* **switch**

	范围匹配
		
		switch count{
			case:0
			case:1...3
			case:4...999_999
		}
		
	元组
		
		swicth somePoint{
			case(0,0):
				println("0,0")
			case(_,0):
				println("_,0")
			case(-2...2,-2...3)
				println("")
			deafault:
				println("eles")
		}

	值绑定
	
		swich	anotherPoint{
			case (let x,0):
			case (0,let y):
		}
		
	where
		
		swich	yetAnotherPoint{
			case let(x,y) where x==y:
			case let(x,y) where x==-y:
		}

* **控制转移语句**

	- continue
	- break
	- fallthrough
		在switch语句中使用，作用是继续匹配下面的条件
	- return
	
##函数

* 函数定义

	有返回值
	
		fun say(name:String)->String{
			return "Hello "+name
		}

	无返回值
		
		fun say(name:String){
			print("Hello "+name)
		}


* 函数形参名

	指定外部形参名
			
		fun someFunction(exernalParameterName localParameterName:Int){
			//foo
		}			
		someFunction(exernalParameterName : 1)

	快速定义形参
	
				
		fun someFunction(#localParameterName:Int){
			//foo
		}			
		someFunction(localParameterName:Int : 1)			
	
* 可变形参

		fun add(numbers:Double...){
			var total:Double
			for number in number{
				total+=number
			}
			return total
		}
		
		add(1,2,3,4,5)
		
* 常量形参&变量形参

	**函数形参默认是常量，试图在函数体被改变函数形参的值会引发一个编译异常**
	
	指定形参为变量形参使用var关键字
		
		fun foo(var str:String)->String{
			str+="123";
			return str
		]

	inout形参（类似引用传递）
	
		fun swap(inout a:Int,inout b:Int){
			let temp=a
			a=b
			b=temp
		}
		
* 函数类型

	使用函数类型
	
		fun add（a:Int,b:Int）->Int{
			return a+b
		}	
	
		var addFunction:(Int,Int)->Int=add
		
	使用函数类型作为形参
	
		fun foo(addFunction:(Int,Int)->Int,a:Int,b:Int){
			print (addFunction(a,b))
		}
		
	作为返回值
		
		fun foo()->(Int,Int)->Int{
			return addFunction
		}
		
	嵌套函数

		fun foo()->(Int,Int)->Int{
			fun anotherFunction(a:Int,b:Int)->Int{
				return a+b
			}
			return anotherFunction
		}
	
##闭包

* 闭包表达式
		
		func backwards (s1:String,s2:String)->Bool{
			return s1>s2
		}
		var reversed=sort(names,backwards)

	闭包表达式语法
		
	*格式:*	
		
		{（parameters）-> returnType in 
			//statements
		}		
		
	上面的方法改写形式
		
		var reversed=sort(names,{(s1:String,s2:String)->Bool in 
			return a1>s2
		})

	根据上下文判断类型
	
		var reversed=sort(names,{s1,s2 in return a1>s2})
		
	单行表达式闭包可以省略return
	
		var reversed = sort(names,{s1,s2 in s1 > s2})
		
	参数名简写
	
	*通过$0,$1,$2等名字来引用闭包的参数值*
		
		var reversed = sort(names,{$0 > $1})

	运算符函数
	
		var reversed = sort(names , >)
		
* Trailing闭包

		var reversed = sort(names){$0 > $1}
		
* 捕获（Caputure）

		func makeIncrementor(forIncrement amount:Int) -> ()->Int{
			var total=0;
			func incrementor()->Int{
				total+=amount
				return total
			}
			return incrementor
		}	

##枚举

* 枚举定义
	
		enum CompassPoint {
			case North
			case South
			case East
			case West
		}
		
	*or*
		
		enum Planet{
			case Mercury,Benus,Earth,Mars,Jupiter,Saturn,Uranus,Nepturn
		}
* 使用

		var directionToHead = CompassPoint.West
		//change value
		directionToHead = .East
		
* 关联值

		enum Barcode{
			case UPCA(Int,Int,Int)
			case QRCode(String)
		}
		
		var productBarcode = Barcode.UPCA(8,634423,3)
		
* 原始值

		enum ASCIIControlCharacter : Character{
			case Tab = "\t"
			case LineFeed = "\n"
			case CarriageReturn = "\r"
		}		

		enum Planet : Int{
			case Mercury:1,Benus,Earth,Mars,Jupiter,Saturn,Uranus,Nepturn
		}
	`Benus`的值将为2
	
		let earthsOrder = Planet.Earth.toRaw()
		//earthsOrder is 3
		
		let possiblePlanet = Planet.fromRaw(7)
		//possiblePlanet is of type Planet and equals Planet.Uranus
		
##类和结构体

		
		
		
		


	



	












		
		
		
		
		
		
		
		



























