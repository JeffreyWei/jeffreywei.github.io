---
layout: post
title: java-concurrent
share: true
comments: true
imagefeature:
tags: [java,concurrent]
category: java,concurrent
description: "java,concurrent"
---


<!--more-->

## 线程应用

使用两种不同的方式实现线程等待：

```java
	public static void main(String[] args) {
		Thread thread=new Thread(()->{
			try {
				Thread.sleep(2000);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		});
		long startTime = System.currentTimeMillis();
		thread.start();
		try {
			thread.join();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println((System.currentTimeMillis()-startTime)/1000);
	}

```

```java
public static void main(String[] args) {
		Object obj = new Object();
		Thread thread = new Thread(() -> {
			try {
				Thread.sleep(2000);
				synchronized (obj) {//需要获得锁才能执行notify、wait,否则会有java.lang.IllegalMonitorStateException异常
					obj.notify();
				}
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		});
		long startTime = System.currentTimeMillis();
		thread.start();
		try {
			synchronized (obj) {
				obj.wait();
			}
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println((System.currentTimeMillis() - startTime) / 1000);
	}

```

## Lock、Condition

```java
public class LockDemo {
	public static void main(String[] args) {
		final Factory factory = new Factory(100);
		Thread threadProvide = new Thread(new Runnable() {
			public void run() {
				factory.provide();
			}
		});
		Thread threadConsume = new Thread(new Runnable() {
			public void run() {
				factory.consume();
			}
		});
		threadProvide.setDaemon(false);
		threadConsume.setDaemon(false);
		threadProvide.start();
		threadConsume.start();
	}
}

@Log
class Factory {
	private Lock lock = new ReentrantLock();
	private Condition put = lock.newCondition();
	private Condition get = lock.newCondition();
	private int capacity;
	private int currentCapacity;

	public Factory(int capacity) {
		this.capacity = capacity;
	}


	public void provide() {
		while (true) {
			try {
				lock.lock();
				if (currentCapacity >= capacity) {
					put.await();
				}
				currentCapacity++;
				log.info("生产");
				get.signal();
			} catch (Exception e) {
				e.printStackTrace();
			} finally {
				lock.unlock();
			}
		}
	}

	public void consume() {
		while (true) {
			try {
				lock.lock();
				if (currentCapacity == 0) {
					get.await();
				}
				currentCapacity--;
				log.info("消费");
				put.signal();
			} catch (Exception e) {
				e.printStackTrace();
			} finally {
				lock.unlock();
			}
		}
	}
}


```

## ForkJoinTask

本地并行处理框架,使用ForkJoinPool、RecursiveTask<T> 

```java
import lombok.extern.java.Log;

import java.util.Random;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.RecursiveTask;

@Log
public class ForkJoinTaskDemo {
	public static void main(String[] args) throws ExecutionException, InterruptedException {
		int[] data = generateArray(1000);
		MaxTask maxTask = new MaxTask(data, 0, 999);
		ForkJoinPool forkJoinPool = new ForkJoinPool();
		forkJoinPool.submit(maxTask);
		Integer max = maxTask.get();
		log.info("最大值:" + max);
	}

	private static int[] generateArray(int size) {
		int[] data = new int[size];
		for (int i = 0; i < size; i++) {
			data[i] = new Random().nextInt();
		}
		return data;
	}
}

class MaxTask extends RecursiveTask<Integer> {

	private final int poolSize = 10;
	private int start;
	private int end;
	private int[] data;

	public MaxTask(int[] data, int start, int end) {
		this.data = data;
		this.start = start;
		this.end = end;
	}

	protected Integer compute() {
		int max = 0;
		if (end - start <= poolSize) {
			for (int i = start; i < end; i++) {
				if (max < data[i]) {
					max = data[i];
				}
			}
		} else {
			int mid = (start + end) / 2;
			MaxTask one = new MaxTask(data, mid + 1, end);
			MaxTask two = new MaxTask(data, start, mid);
			one.fork();
			two.fork();
			int oneResult = one.join();
			int twoResult = two.join();
			max = oneResult > twoResult ? oneResult : twoResult;
		}
		return max;
	}
}

```