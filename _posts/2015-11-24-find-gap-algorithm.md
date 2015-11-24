---
layout: post
title: 寻找数轴上相邻两个数之间的最大差值
share: true
comments: true
imagefeature:
tags: [python, algorithm]
category: python, algorithm
description: "N个未排序的整数，在线性时间内，求这N个整数在数轴上相邻两个数之间的最大差值"
---

Via python

<!--more-->

中午在网上看到的一道面试题，最近再补Python就试着写了下

**问题描述：N个未排序的整数，在线性时间内，求这N个整数在数轴上相邻两个数之间的最大差值**

{% highlight java %}

#!/usr/bin/env python
# -*- coding:utf-8 -*-
__author__ = 'wei'


def interval(array):
	"每次找到一组数中相对小的，距离最近，在所有集合中找到最大的即为结果"
	gap = []
	for index, i in enumerate(array):
		set = []
		for j in array[index + 1:]:
			set.append(abs(i - j))
		if (len(set) != 0):
			gap.append(min(set))
	return max(gap)


if __name__ == '__main__':
	array = [6, 45, 23, 4, 1, -2]
	print(interval(array))


{%  endhighlight %}