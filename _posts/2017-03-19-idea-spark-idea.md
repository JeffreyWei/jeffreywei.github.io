---
layout: post
title: idea中配置spark开发环境via scala
share: true
comments: true
imagefeature:
tags: [idea,spark,scala]
category: bigdata
description: ""
---



<!--more-->



## 具体步骤

* 为idea添加scala插件
* 下载spark,查看`jars`中scala版本，如：*scala-compiler-2.11.8.jar*、*scala-library-2.11.8.jar*
* 下载上面对应的scala版本
* idea中添加scalaSDK,导入spark依赖

demo code

	import org.apache.spark.{SparkConf, SparkContext}

	/**
	  * Created by wei on 17/3/19.
	  */
	object Demo extends App {
	  val conf = new SparkConf().setAppName("hello").setMaster("local")
	  val sc = new SparkContext(conf)
	  var text = sc.parallelize(Seq("a", "b", "c"))
	  val c = text.count()
	  print(c)
	}



## 遇到的问题

1. 项目配置


		Error:scalac: Error: requirement failed: package compress
			java.lang.IllegalArgumentException: requirement failed: package compress
			at scala.reflect.internal.Types$ModuleTypeRef.<init>(Types.scala:1879)
			
需要修改项目依赖及SDK设置

2. 缺少依赖

		Error:scalac: error while loading RDD, Missing dependency 'bad symbolic reference. A signature in RDD.class refers to term annotation
		in package org.apache.spark which is not available.
	
	
查看`spark-core_2.11-2.1.0.jar`确实有RDD这个类，尝试把`jars`目录下的所有jar导入后运行正常


上面的两个问题困扰了我很久，直到[Error in running Spark-Scala example in intellij
](http://stackoverflow.com/questions/41098631/error-in-running-spark-scala-example-in-intellij)，里面提到了scala版本，查看spark的依赖里面已经包含了scala,但如果要在idea中启动又必须要设置一个SDK,又下载了spark版本一直的scala后才启动正常。
