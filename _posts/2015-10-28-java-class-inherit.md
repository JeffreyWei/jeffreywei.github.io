---
layout: post
title: java类继承中的一些规则
share: true
comments: true
imagefeature:
tags: [java]
category: java
description: "java class inherit"
---

子类初始化的顺序

<!--more-->
## 子类初始化时构造函数优于类中的字段

{% highlight java %}
public class A {
	private int mSuperX;
	public A() {
		setX(99);
	}
	public static void main(String[] args) {
		B sc = new B();
		sc.printX();
	}
	public void setX(int x) {
		mSuperX = x;
	}
}

class B extends A {
	private int mSubX = 1;
	public B() {
	}
	@Override
	public void setX(int x) {
		super.setX(x);
		mSubX = x;
		System.out.println("SubX is assigned " + x);
	}
	public void printX() {
		System.out.println("SubX = " + mSubX);
	}
}
{%  endhighlight %}

输出结果为

	SubX is assigned 99
	1

构造子类B需要先是与偶那个A的构造函数，由于`setX`被重写，然后执行了B中的`setX`方法输出99，然后初始化B中的字段`mSubX = 1`，完成B对象的初始化。


**构造子类时会先调用父类的无参构造方法，即使调用的是子类的有参构造方法**

{% highlight java %}
public class A {
	private int mSuperX;
	public A() {
		System.out.println("AAAAA");
	}
	public static void main(String[] args) {
		C sc = new C(1);
	}
	public A (int x) {
		System.out.println("A1");
	}
}
class B extends A {
	private int mSubX = 1;
	public B() {
		System.out.println("BBBBBBB");
	}
	public void printX() {
		System.out.println("SubX = " + mSubX);
	}
	public B (int x) {
		System.out.println("B1");
	}
}
class C extends B{
	public C(){
		System.out.println("CCCCCC");
	}
	public C (int x) {
		System.out.println("C1");
	}
}
{% endghighlight %}
输出结果为

	
	AAAAA
	BBBBBBB
	C1



