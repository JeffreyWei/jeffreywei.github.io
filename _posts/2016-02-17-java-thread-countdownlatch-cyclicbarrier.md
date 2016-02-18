---
layout: post
title: 线程协作CountDownLatch&CyclicBarrier
share: true
comments: true
imagefeature:
tags: [java,thread]
category: java,thread
description: "using CountDownLatch&CyclicBarrier"
---

最近在开发计算量较大的模块中使用到的线程协作总结

<!--more-->

## 需求
要求三个线程按下图所示的流程协同执行

![][1]

## 实现

{% highlight java %}

public class ThreadDemo {
	public static void main(String[] args) {
		final CountDownLatch countDownLatch = new CountDownLatch(2);
		final CyclicBarrier cyclicBarrier = new CyclicBarrier(2);
		Thread threadA = new Thread(new worker("A", countDownLatch, cyclicBarrier));
		Thread threadB = new Thread(new worker("B", countDownLatch, cyclicBarrier));
		threadB.start();
		threadA.start();
		try {
			countDownLatch.await();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println("All worke done");
	}

	public static void threadSleep() {
		try {
			Thread.sleep(2000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}

	}
}

class worker implements Runnable {
	private String workerNmae;
	private CountDownLatch countDownLatch;
	private CyclicBarrier cyclicBarrier;

	public worker(String workerNmae, CountDownLatch countDownLatch, CyclicBarrier cyclicBarrier) {
		this.workerNmae = workerNmae;
		this.countDownLatch = countDownLatch;
		this.cyclicBarrier = cyclicBarrier;
	}

	public void run() {
		System.out.println(this.workerNmae + " start work");
		ThreadDemo.threadSleep();
		try {
			this.cyclicBarrier.await();
		} catch (InterruptedException e) {
			e.printStackTrace();
		} catch (BrokenBarrierException e) {
			e.printStackTrace();
		}
		ThreadDemo.threadSleep();
		System.out.println(this.workerNmae + " end work");
		countDownLatch.countDown();
	}
}

{%  endhighlight %}



## 区别

不难看出，这两个对象使用的时机、方式有所不同，`CyclicBarrier`的await使工作线程阻塞；`CountDownLatch`的await在需要并行节点结束时使用，达到线程协作。

[1]: {{ site.url }}/assets/posts/2016-02/1.png "流程图"


