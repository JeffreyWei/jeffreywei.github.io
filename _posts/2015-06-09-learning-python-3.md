---
layout: post
title: python重点回顾（3）
share: false
comments: true
imagefeature:
tags: [python]
category: python
description: "Learning python 3"
---

python教程重点回顾-3-错误、调试、测试

<!--more-->

##错误、调试、测试

###错误处理
{% highlight python %}
try:
    print 'try...'
    r = 10 / int('a')
    print 'result:', r
except ValueError, e:
    print 'ValueError:', e
except ZeroDivisionError, e:
    print 'ZeroDivisionError:', e
finally:
    print 'finally...'
print 'END'


{%  endhighlight %}

###异常抛出

{% highlight python %}
# err.py
class FooError(StandardError):
    pass

def foo(s):
    n = int(s)
    if n==0:
        raise ValueError('invalid value: %s' % s)
    return 10 / n
{%  endhighlight %}
###调试

* 使用断言

	assert n != 0, 'n is zero!


	
* 使用log
	
	使用`import logging`,调用方法`logging.info()`



###单元测试
为了编写单元测试，我们需要引入Python自带的`unittest`模块，编写mydict_test.py如下：
{% highlight python %}
import unittest

from mydict import Dict

class TestDict(unittest.TestCase):

    def test_init(self):
        d = Dict(a=1, b='test')
        self.assertEquals(d.a, 1)
        self.assertEquals(d.b, 'test')
        self.assertTrue(isinstance(d, dict))

    def test_key(self):
        d = Dict()
        d['key'] = 'value'
        self.assertEquals(d.key, 'value')

    def test_attr(self):
        d = Dict()
        d.key = 'value'
        self.assertTrue('key' in d)
        self.assertEquals(d['key'], 'value')

    def test_keyerror(self):
        d = Dict()
        with self.assertRaises(KeyError):
            value = d['empty']

    def test_attrerror(self):
        d = Dict()
        with self.assertRaises(AttributeError):
            value = d.empty

{%  endhighlight %}

运行单元测试

{% highlight python %}
if __name__ == '__main__':
    unittest.main()

$ python mydict_test.py
#结果
$ python -m unittest mydict_test
.....
----------------------------------------------------------------------
Ran 5 tests in 0.000s

OK
{%  endhighlight %}

可以在单元测试中编写两个特殊的`setUp()`和`tearDown()`方法。这两个方法会分别在`每调用一个测试方法`的`前后`分别被执行。
{% highlight python %}
class TestDict(unittest.TestCase):

    def setUp(self):
        print 'setUp...'

    def tearDown(self):
        print 'tearDown...'
{%  endhighlight %}

###文档测试
Python内置的“文档测试”（doctest）模块可以直接提取注释中的代码并执行测试。






















{% highlight python %}
class Dict(dict):
    '''
    Simple dict but also support access as x.y style.

    >>> d1 = Dict()
    >>> d1['x'] = 100
    >>> d1.x
    100
    >>> d1.y = 200
    >>> d1['y']
    200
    >>> d2 = Dict(a=1, b=2, c='3')
    >>> d2.c
    '3'
    >>> d2['empty']
    Traceback (most recent call last):
        ...
    KeyError: 'empty'
    >>> d2.empty
    Traceback (most recent call last):
        ...
    AttributeError: 'Dict' object has no attribute 'empty'
    '''
    def __init__(self, **kw):
        super(Dict, self).__init__(**kw)

    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Dict' object has no attribute '%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value

if __name__=='__main__':
    import doctest
    doctest.testmod()


#运行python mydict.py
$ python mydict.py    
{%  endhighlight %}
doctest非常有用，不但可以用来测试，还可以直接作为示例代码。通过某些文档生成工具，就可以自动把包含doctest的注释提取出来。用户看文档的时候，同时也看到了doctest。

参考：[python教程](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000)


