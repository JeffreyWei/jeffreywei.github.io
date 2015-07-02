---
layout: post
title: python重点回顾（5）
share: false
comments: true
imagefeature:
tags: [python]
category: python
description: "Learning python 5"
---

python教程重点回顾-5-常用内建模块

<!--more-->

##常用内建模块
###collections

1.namedtuple，命名元组

{% highlight python %}
>>> from collections import namedtuple
>>> Point = namedtuple('Point', ['x', 'y'])
>>> p = Point(1, 2)
>>> p.x
1
>>> p.y
2

>>> isinstance(p, Point)
True
>>> isinstance(p, tuple)
True

# namedtuple('名称', [属性list]):
Circle = namedtuple('Circle', ['x', 'y', 'r'])
{%  endhighlight %}


2. deque,双向列表

使用list存储数据时，按索引访问元素很快，但是插入和删除元素就很慢了，因为`list是线性存储`，数据量大的时候，插入和删除效率很低。`deque`是为了高效实现`插入和删除操作的双向列表`，适合用于队列和栈：

{% highlight python %}
>>> from collections import deque
>>> q = deque(['a', 'b', 'c'])
>>> q.append('x')
>>> q.appendleft('y')
>>> q
deque(['y', 'a', 'b', 'c', 'x'])
{%  endhighlight %}
deque除了实现list的append()和pop()外，还支持appendleft()和popleft()，这样就可以非常高效地往头部添加或删除元素。


3.defaultdict,含默认值字典

{% highlight python %}
使用dict时，如果引用的Key不存在，就会抛出KeyError。如果希望key不存在时，返回一个默认值，就可以用defaultdict：
>>> from collections import defaultdict
>>> dd = defaultdict(lambda: 'N/A')
>>> dd['key1'] = 'abc'
>>> dd['key1'] # key1存在
'abc'
>>> dd['key2'] # key2不存在，返回默认值
'N/A'

{%  endhighlight %}

4.OrderedDict，有序字典

使用dict时，Key是无序的。在对dict做迭代时，如果要`保持Key的顺序`，可以用`OrderedDict`：
{% highlight python %}
>>> from collections import OrderedDict
>>> d = dict([('a', 1), ('b', 2), ('c', 3)])
>>> d # dict的Key是无序的
{'a': 1, 'c': 3, 'b': 2}
>>> od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
>>> od # OrderedDict的Key是有序的
OrderedDict([('a', 1), ('b', 2), ('c', 3)])

#注意，OrderedDict的Key会按照插入的顺序排列，不是Key本身排序：
>>> od = OrderedDict()
>>> od['z'] = 1
>>> od['y'] = 2
>>> od['x'] = 3
>>> od.keys() # 按照插入的Key的顺序返回
['z', 'y', 'x']
{%  endhighlight %}

5.Counter,计数器

{% highlight python %}
>>> from collections import Counter
>>> c = Counter()
>>> for ch in 'programming':
...     c[ch] = c[ch] + 1
...
>>> c
Counter({'g': 2, 'm': 2, 'r': 2, 'a': 1, 'i': 1, 'o': 1, 'n': 1, 'p': 1})
{%  endhighlight %}


###hashlib
1.MD5

{% highlight python %}
import hashlib
md5 = hashlib.md5()
md5.update('how to use md5 in python hashlib?')
print md5.hexdigest()


#如果数据量很大，可以分块多次调用update()，最后计算的结果是一样的
md5 = hashlib.md5()
md5.update('how to use md5 in ')
md5.update('python hashlib?')
print md5.hexdigest()
{%  endhighlight %}


2.SHA1

{% highlight python %}
import hashlib

sha1 = hashlib.sha1()
sha1.update('how to use sha1 in ')
sha1.update('python hashlib?')
print sha1.hexdigest()
{%  endhighlight %}


###itertools
Python的内建模块`itertools`提供了非常有用的用于操作迭代对象的函数
{% highlight python %}
>>> import itertools
>>> natuals = itertools.count(1)
>>> for n in natuals:
...     print n
...
1
2
3
...
{%  endhighlight %}
因为count()会创建一个无限的迭代器，所以上述代码会打印出自然数序列，根本停不下来，只能按Ctrl+C退出

cycle()会把传入的一个序列无限重复下去：

{% highlight python %}
>>> import itertools
>>> cs = itertools.cycle('ABC') # 注意字符串也是序列的一种
>>> for c in cs:
...     print c
...
'A'
'B'
'C'
'A'
'B'
'C'
...


#repeat()负责把一个元素无限重复下去，不过如果提供第二个参数就可以限定重复次数
>>> ns = itertools.repeat('A', 10)
>>> for n in ns:
...     print n
...
打印10次'A'


#chain()可以把一组迭代对象串联起来，形成一个更大的迭代器：
for c in chain('ABC', 'XYZ'):
    print c
# 迭代效果：'A' 'B' 'C' 'X' 'Y' 'Z'


#groupby()把迭代器中相邻的重复元素挑出来放在一起：
>>> for key, group in itertools.groupby('AAABBBCCAAA'):
...     print key, list(group) # 为什么这里要用list()函数呢？
...
A ['A', 'A', 'A']
B ['B', 'B', 'B']
C ['C', 'C']
A ['A', 'A', 'A']

#imap()和map()的区别在于，imap()可以作用于无穷序列，并且，如果两个序列的长度不一致，以短的那个为准。
>>> for x in itertools.imap(lambda x, y: x * y, [10, 20, 30], itertools.count(1)):
...     print x
...
10
40
90

#imap()返回一个迭代对象，而map()返回list。当你调用map()时，已经计算完毕：
>>> r = map(lambda x: x*x, [1, 2, 3])
>>> r # r已经计算出来了
[1, 4, 9]
{%  endhighlight %}

###XML
当SAX解析器读到一个节点时：

		<a href="/">python</a>

会产生3个事件：

start_element事件，在读取<a href="/">时；

char_data事件，在读取python时；

end_element事件，在读取</a>时。

{% highlight python %}
from xml.parsers.expat import ParserCreate

class DefaultSaxHandler(object):
    def start_element(self, name, attrs):
        print('sax:start_element: %s, attrs: %s' % (name, str(attrs)))

    def end_element(self, name):
        print('sax:end_element: %s' % name)

    def char_data(self, text):
        print('sax:char_data: %s' % text)

xml = r'''<?xml version="1.0"?>
<ol>
    <li><a href="/python">Python</a></li>
    <li><a href="/ruby">Ruby</a></li>
</ol>
'''
handler = DefaultSaxHandler()
parser = ParserCreate()
parser.returns_unicode = True
parser.StartElementHandler = handler.start_element
parser.EndElementHandler = handler.end_element
parser.CharacterDataHandler = handler.char_data
parser.Parse(xml)

{%  endhighlight %}


参考：[python教程](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000)