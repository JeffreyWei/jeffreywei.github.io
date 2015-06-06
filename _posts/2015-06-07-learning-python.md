---
layout: post
title: python重点回顾
share: false
comments: true
imagefeature:
tags: [code,python]
category: code,python
description: "Learning python"
---

python教程重点回顾

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

###安装第三方模块

###使用_future_

###递归函数

{% highlight python %}

{%  endhighlight %}



{% highlight python %}

{%  endhighlight %}



{% highlight python %}

{%  endhighlight %}





{% highlight python %}

{%  endhighlight %}





{% highlight python %}

{%  endhighlight %}





{% highlight python %}

{%  endhighlight %}






{% highlight python %}

{%  endhighlight %}


{% highlight python %}

{%  endhighlight %}

![][1]

[1]: {{ site.url }}/assets/posts/2015-06/06-07-1.jpg "奸商"



























