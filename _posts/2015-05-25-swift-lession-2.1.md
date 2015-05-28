---
layout: post
title: Swift教程第二章(1)
share: true
comments: true
imagefeature:
tags: [swift,work]
category: swift,work
description: "swift language part-2.1"
---

Swift教程第二章(1)--Swift基础
* Swift基础
* 基本运算符
* 流程控制
* 函数


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
	

		
		
		
		


	



	












		
		
		
		
		
		
		
		



























