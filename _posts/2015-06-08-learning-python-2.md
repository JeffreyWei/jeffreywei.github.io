---
layout: post
title: python重点回顾（2）
share: false
comments: true
imagefeature:
tags: [python]
category: python
description: "Learning python 2"
---

python教程重点回顾-2-面向对象的编程

<!--more-->

##面向对象高级编程

###使用_slots_
因为动态绑定的原因，如果需要在类的定义中添加限制对某些对象的添加可以使用`_slots_`

{% highlight python %}
>>> class Student(object):
...     __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称


>>> s = Student() # 创建新的实例
>>> s.name = 'Michael' # 绑定属性'name'
>>> s.age = 25 # 绑定属性'age'
>>> s.score = 99 # 绑定属性'score'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
{%  endhighlight %}
这里只允许添加`name`,`age`属性

###使用@property
类似于其他高级语言中的get、set
{% highlight python %}
class Student(object):

    @property
    def birth(self):
        return self._birth

    @birth.setter
    def birth(self, value):
        self._birth = value

    @property
    def age(self):
        return 2014 - self._birth
{%  endhighlight %}
上面的`birth`是可读写属性，而`age`就是一个只读属性

###多重继承
python支持多重继承
{% highlight python %}
class Dog(Mammal, Runnable):
    pass
{%  endhighlight %}

###定制类

* __str__,类似于重写toString方法

{% highlight python %}
>>> class Student(object):
...     def __init__(self, name):
...         self.name = name
...     def __str__(self):
...         return 'Student object (name: %s)' % self.name
...
>>> print Student('Michael')
Student object (name: Michael)
{%  endhighlight %}

* __iter__

	如果一个类想被用于for ... in循环，类似list或tuple那样，就必须实现一个__iter__()方法，该方法返回一个迭代对象，然后，Python的for循环就会不断调用该迭代对象的next()方法拿到循环的下一个值，直到遇到StopIteration错误时退出循环。
{% highlight python %}
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1 # 初始化两个计数器a，b

    def __iter__(self):
        return self # 实例本身就是迭代对象，故返回自己

    def next(self):
        self.a, self.b = self.b, self.a + self.b # 计算下一个值
        if self.a > 100000: # 退出循环的条件
            raise StopIteration();
        return self.a # 返回下一个值

>>> for n in Fib():
...     print n
...
1
1
2
3
5
...
46368
75025      
{%  endhighlight %}

* __getitem__,使用属性访问器的方式访问对象内的元素

{% highlight python %}
class Fib(object):
    def __getitem__(self, n):
        a, b = 1, 1
        for x in range(n):
            a, b = b, a + b
        return a

>>> f = Fib()
>>> f[0]
1
>>> f[1]
1
>>> f[2]
2        
{%  endhighlight %}

* __getattr__，调用属性前执行的方法

{% highlight python %}
class Student(object):

    def __init__(self):
        self.name = 'Michael'

    def __getattr__(self, attr):
        if attr=='score':
            return 99

#对象并不包含score属性，如果没有__getattr__方法会直接报错
>>> s = Student()
>>> s.name
'Michael'
>>> s.score
99        
{%  endhighlight %}

* __call__，使用`obj()`直接调用对象方法


{% highlight python %}
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('My name is %s.' % self.name)

>>> s = Student('Michael')
>>> s()
My name is Michael.


#使用callable()检查对象是否可调用对象方法
>>> callable(Student())
True
>>> callable(max)
True
>>> callable([1, 2, 3])
False
>>> callable(None)
False
>>> callable('string')
False        
{%  endhighlight %}

###使用元类

* type()查看函数、类、对象类型

{% highlight python %}
class Hello(object):
    def hello(self, name='world'):
        print('Hello, %s.' % name)

>>> from hello import Hello
>>> h = Hello()
>>> h.hello()
Hello, world.
>>> print(type(Hello))
<type 'type'>
>>> print(type(h))
<class 'hello.Hello'>        
{%  endhighlight %}

使用`type()`方法创建对象，而不用定义类

{% highlight python %}
>>> def fn(self, name='world'): # 先定义函数
...     print('Hello, %s.' % name)
...
>>> Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class
>>> h = Hello()
>>> h.hello()
Hello, world.
>>> print(type(Hello))
<type 'type'>
>>> print(type(h))
<class '__main__.Hello'>
{%  endhighlight %}

要创建一个class对象，type()函数依次传入3个参数：

1.class的名称；
2.继承的父类集合，注意Python支持多重继承，如果只有一个父类，别忘了tuple的单元素写法；
3.class的方法名称与函数绑定，这里我们把函数`fn`绑定到方法名`hello`上。

* metaclass

类可以看成是metaclass创建出来的“实例”

为自定义的类添加一个`add`方法
{% highlight python %}
# metaclass是创建类，所以必须从`type`类型派生：
class ListMetaclass(type):
    def __new__(cls, name, bases, attrs):
        attrs['add'] = lambda self, value: self.append(value)
        return type.__new__(cls, name, bases, attrs)

class MyList(list):
    __metaclass__ = ListMetaclass # 指示使用ListMetaclass来定制类
{%  endhighlight %}
当我们写下`__metaclass__ = ListMetaclass`语句时，魔术就生效了，它指示Python解释器在创建MyList时，要通过`ListMetaclass.__new__()`来创建，在此，我们可以修改类的定义，比如，加上新的方法，然后，返回修改后的定义。

__new__()方法接收到的参数依次是：

1.当前准备创建的类的对象；

2.类的名字；

3.类继承的父类集合；

4.类的方法集合。

参考：[python教程](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000)
