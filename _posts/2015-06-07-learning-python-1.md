---
layout: post
title: python重点回顾（1）
share: false
comments: true
imagefeature:
tags: [code,python]
category: code,python
description: "Learning python 1"
---

python教程重点回顾-1-python基础

<!--more-->

##python基础

{% highlight python %}

# 输出
print 'hello','world'
# `,`相当于一个空格

# 获取键盘输入的字符串
string = raw_input("please input: ")

# 循环
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print name

sum = 0
for x in range(101):
    sum = sum + x
print sum

# set
>>> s = set([1, 1, 2, 2, 3, 3])
>>> s
set([1, 2, 3])
{%  endhighlight %}

##函数
{% highlight python %}
# 函数定义
# `*`为元组类型，`**`为字典类型
def func(a, b, c=0, *args, **kw):
    print 'a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw

>>> args = (1, 2, 3, 4)
>>> kw = {'x': 99}
>>> func(*args, **kw)
a = 1 b = 2 c = 3 args = (4,) kw = {'x': 99}


{%  endhighlight %}

##高级功能
###切片
{% highlight python %}
>>> L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
>>> L[0:3]
['Michael', 'Sarah', 'Tracy']
>>> L[1:3]
['Sarah', 'Tracy']
>>> L[-2:]
['Bob', 'Jack']
>>> L[-2:-1]
['Bob']


>>> L = range(100)
>>> L
[0, 1, 2, 3, ..., 99]
>>> L[:10]
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> L[-10:]
[90, 91, 92, 93, 94, 95, 96, 97, 98, 99]

>>> 'ABCDEFG'[:3]
'ABC'
>>> 'ABCDEFG'[::2]
'ACEG'
{%  endhighlight %}
###迭代

{% highlight python %}
#判断是否可迭代
>>> from collections import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False

>>> for i, value in enumerate(['A', 'B', 'C']):
...     print i, value
...
0 A
1 B
2 C

>>> for x, y in [(1, 1), (2, 4), (3, 9)]:
...     print x, y
...
1 1
2 4
3 9
{%  endhighlight %}

###列表生成


{% highlight python %}
#生成[1x1, 2x2, 3x3, ..., 10x10]
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]


>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']


>>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> for k, v in d.iteritems():
...     print k, '=', v
... 
y = B
x = A
z = C


>>> L = ['Hello', 'World', 'IBM', 'Apple']
>>> [s.lower() for s in L]
['hello', 'world', 'ibm', 'apple']
{%  endhighlight %}

###生成器

{% highlight python %}
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x104feab40>

>>> g.next()
0
>>> g.next()
1
#next方法很少用，更多使用for访问其中的元素
>>> g = (x * x for x in range(10))
>>> for n in g:
...     print n
...
0
1

#迭代函数，把返回值改为`yield`
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
{%  endhighlight %}

##函数式编程
###map/reduce

* 定义map
{% highlight python %}
>>> def f(x):
...     return x * x
...
>>> map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
[1, 4, 9, 16, 25, 36, 49, 64, 81]
{%  endhighlight %}

* 定义reduce

`reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)`
{% highlight python %}
>>> def add(x, y):
...     return x + y
...
>>> reduce(add, [1, 3, 5, 7, 9])
25

{%  endhighlight %}

* 例子

{% highlight python %}

def str2int(s):
    def fn(x, y):
        return x * 10 + y
    def char2num(s):
        return {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}[s]
    return reduce(fn, map(char2num, s))
{%  endhighlight %}
还可以用lambda函数进一步简化成：
{% highlight python %}

def char2num(s):
    return {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}[s]

def str2int(s):
    return reduce(lambda x,y: x*10+y, map(char2num, s))
{%  endhighlight %}

###filter
{% highlight python %}
def is_odd(n):
    return n % 2 == 1

filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15])
# 结果: [1, 5, 9, 15]

# 删除空值
def not_empty(s):
    return s and s.strip()

filter(not_empty, ['A', '', 'B', None, 'C', '  '])
# 结果: ['A', 'B', 'C']
{%  endhighlight %}

###sorted


{% highlight python %}
#基本用法
>>> sorted([36, 5, 12, 9, 21])
[5, 9, 12, 21, 36]


#自定义
def reversed_cmp(x, y):
    if x > y:
        return -1
    if x < y:
        return 1
    return 0
>>> sorted([36, 5, 12, 9, 21], reversed_cmp)
[36, 21, 12, 9, 5]
{%  endhighlight %}

###延迟计算，闭包
{% highlight python %}
#返回延迟执行的函数
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum

#注每次返回得到的时一个新的函数
>>> f1 = lazy_sum(1, 3, 5, 7, 9)
>>> f2 = lazy_sum(1, 3, 5, 7, 9)
>>> f1==f2
False


#闭包
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()
>>> f1()
9
>>> f2()
9
>>> f3()
9
{%  endhighlight %}

###匿名函数
{% highlight python %}
>>> map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9])
[1, 4, 9, 16, 25, 36, 49, 64, 81]
#lambda x: x * x实际上就是：
def f(x):
    return x * x


>>> f = lambda x: x * x
>>> f
<function <lambda> at 0x10453d7d0>
>>> f(5)
25
{%  endhighlight %}

###装饰器

{% highlight python %}
def log(func):
    def wrapper(*args, **kw):
        print 'call %s():' % func.__name__
        return func(*args, **kw)
    return wrapper
@log
def now():
    print '2013-12-25'

>>> now()
call now():
2013-12-25

#把@log放到now()函数的定义处，相当于执行了语句：
#now = log(now)
{%  endhighlight %}


##模块
###使用模块
{% highlight python %}
#!/usr/bin/env python
# -*- coding: utf-8 -*-

' a test module '

__author__ = 'Michael Liao'

import sys

def test():
    args = sys.argv
    if len(args)==1:
        print 'Hello, world!'
    elif len(args)==2:
        print 'Hello, %s!' % args[1]
    else:
        print 'Too many arguments!'

if __name__=='__main__':
    test()
{%  endhighlight %}

第1行和第2行是标准注释，第1行注释可以让这个hello.py文件直接在Unix/Linux/Mac上运行，第2行注释表示.py文件本身使用标准UTF-8编码；

第4行是一个字符串，表示模块的文档注释，任何模块代码的第一个字符串都被视为模块的文档注释；

第6行使用`__author__`变量把作者写进去，这样当你公开源代码后别人就可以瞻仰你的大名；

当我们在命令行运行hello模块文件时，Python解释器把一个特殊变量`__name__`置为`__main__`，而如果在其他地方导入该`hello`模块时，if判断将失败，因此，这种if测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试。

导入模块时，还可以使用别名，这样，可以在运行时根据当前环境选择最合适的模块。比如Python标准库一般会提供`StringIO`和`cStringIO`两个库，这两个库的接口和功能是一样的，但是cStringIO是C写的，速度更快，所以，你会经常看到这样的写法：


{% highlight python %}
try:
    import cStringIO as StringIO
except ImportError: # 导入失败会捕获到ImportError
    import StringIO
{%  endhighlight %}

###模块中的作用域
在一个模块中，我们可能会定义很多函数和变量，但有的函数和变量我们希望给别人使用，有的函数和变量我们希望仅仅在模块内部使用。在Python中，是通过_前缀来实现的。

正常的函数和变量名是公开的`public`，可以被直接引用，比如：`abc`，`x123`，`PI`等；

类似__xxx__这样的变量是特殊变量，可以被直接引用，但是有特殊用途，比如上面的`__author__`，`__name__`就是特殊变量，hello模块定义的文档注释也可以用特殊变量`__doc__`访问，我们自己的变量一般不要用这种变量名；

类似_xxx和__xxx这样的函数或变量就是非公开的`private`，不应该被直接引用，比如`_abc`，`__abc`等；


###安装第三方模块
在Python中，安装第三方模块，是通过setuptools这个工具完成的。Python有两个封装了setuptools的包管理工具：`easy_install`和`pip`。目前官方推荐使用`pip`。

使用：
{% highlight python %}
>>> import sys
>>> sys.path
{%  endhighlight %}

查看当前目录、所有已安装的内置模块和第三方模块

###使用_future_
Python提供了__future__模块，把下一个新版本的特性导入到当前版本。

##面向对象编程
###类和实例
* 类定义

{% highlight python %}
class Student (object):
	pass
{%  endhighlight %}
其中`object`为父类

* 实例化

{% highlight python %}
bart = Student()
{%  endhighlight %}

* 类模板

{% highlight python %}
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

{%  endhighlight %}
注意到`__init__`方法的第一个参数永远是`self`，表示创建的实例本身，因此，在`__init__`方法内部，就可以把各种属性绑定到`self`，因为`self`就指向创建的实例本身。

和静态语言不同，Python允许对实例变量绑定任何数据，也就是说，对于两个实例变量，虽然它们都是同一个类的不同实例，但拥有的变量名称都可能不同
{% highlight python %}
>>> bart = Student('Bart Simpson', 59)
>>> lisa = Student('Lisa Simpson', 87)
>>> bart.age = 8
>>> bart.age
8
>>> lisa.age
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'age'
{%  endhighlight %}
`age`属性不是所有实例都有的
###访问限制
如果希望内部属性为私有，内部属性定义时使用`_`前缀
###集成和多态
{% highlight python %}
#父类
class Animal(object):
    def run(self):
        print 'Animal is running...'
#子类
class Dog(Animal):
    def run(self):
        print 'Dog is running...'

class Cat(Animal):
    def run(self):
        print 'Cat is running...'
{%  endhighlight %}
子类重写了父类的方法，使用`isinstance(obj, type)`判断对象类型
###获取对象信息
使用`type()`获得对象信息
{% highlight python %}
#基础类型
>>> type(123)
<type 'int'>
>>> type('str')
<type 'str'>
>>> type(None)
<type 'NoneType'>
#or
>>> import types
>>> type('abc')==types.StringType
True
>>> type(u'abc')==types.UnicodeType
True
>>> type([])==types.ListType
True
>>> type(str)==types.TypeType
True

#函数或类
>>> type(abs)
<type 'builtin_function_or_method'>
>>> type(a)
<class '__main__.Animal'>

#判断类型是否相同
>>> type(123)==type(456)
True
>>> type('abc')==type('123')
True
>>> type('abc')==type(123)
False


#也可以使用isinstance()
>>> a = Animal()
>>> d = Dog()
>>> h = Husky()
>>> isinstance(h, Husky)
True
{%  endhighlight %}
使用`dir()`获得对象的所有属性和方法

{% highlight python %}
>>> hasattr(obj, 'x') # 有属性'x'吗？
True
>>> obj.x
9
>>> hasattr(obj, 'y') # 有属性'y'吗？
False
>>> setattr(obj, 'y', 19) # 设置一个属性'y'
>>> hasattr(obj, 'y') # 有属性'y'吗？
True
>>> getattr(obj, 'y') # 获取属性'y'
19
>>> obj.y # 获取属性'y'
19


#可以传入一个default参数，如果属性不存在，就返回默认值
>>> getattr(obj, 'z', 404) # 获取属性'z'，如果不存在，返回默认值404
404
{%  endhighlight %}

参考：[python教程](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000)
