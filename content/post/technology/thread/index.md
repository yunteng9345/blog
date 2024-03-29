+++

author = "云腾"
title = "Java多线程详解"
date = "2021-08-23"
tags = [
   
]
categories = [
    "技术",
]

+++

## 何为线程？

> **线程**（thread）是操作系统能够进行运算调度的**最小单位**。它被包含在进程之中，是进程中的实际运作单位。一条线程指的是进程中一个单一顺序的控制流，一个进程中可以并发多个线程，每条线程并行执行不同的任务。

## Java线程的生命周期

![Java线程的生命周期——码艺术](threadLifeCycle.png)

1. 新建（New）：刚使用new方法，new出来的线程。
2. 就绪（Runnable）：调用的线程的`start()`方法后，这时候**线程处于等待CPU分配资源阶段，谁先抢的CPU资源，谁开始执行**。
3. 运行（Running）：当就绪的线程被调度并获得CPU的资源时，便进入了运行状态，run方法定义了线程的功能。
4. 阻塞（Blocked）：在运行状态的时候，可能因为某些原因导致运行状态的线程进入了阻塞状态，比如`sleep()`、`wait()`之后线程就变为阻塞状态，这个时候需要有其他机制将阻塞状态的线程唤醒，比如调用`notify()`或者`notifyAll()`方法，**唤醒的线程不会立即执行`run()`方法**，而是进入就绪状态（Runnable）状态，再次等待CPU分配资源进入运行状态。
5. 销毁（Terminated）：如果线程正常执行完成后或线程被提前强制性终止、出现异常导致结束，那么线程就要被销毁并释放资源。

## 新建状态

`Thread t1 = new Thread();`

这里的创建，仅仅是在JAVA的这种编程语言层面被创建，而在操作系统层面，真正的线程还没有被创建。只有当我们调用了 start() 方法之后，该线程才会被创建出来，进入Runnable状态。只有当我们调用了 `start()` 方法之后，该线程才会被创建出来。

![Java线程的新建状态——码艺术](threadLifeCycle_new.png)

## 就绪状态

`t1.start()`

调用`start()`方法后，JVM 进程会去创建一个新的线程，而此线程不会马上被 CPU 调度运行，进入Running状态，这里会有一个中间状态，就是Runnable状态，可以理解为等待被 CPU 调度的状态。

![Java线程的就绪状态——码艺术](threadLifeCycle_runnable.png)

Runnable状态的线程**无法直接进入Blocked状态和Terminated状态**。只能进入Running状态的线程，换句话说，只有获得CPU调度执行权的线程才有资格进入Blocked状态和Terminated状态，Runnable状态的线程要么能被转换成Running状态，要么被意外终止。如下所示：

![Java线程的就绪状态转变——码艺术](threadLifeCycle_runnable2status.png)

## 运行状态

当CPU调度发生，并从任务队列中选中了某个Runnable线程时，该线程会进入Running执行状态，并且开始调用run()方法中逻辑代码。



处于Running状态的线程能发生以下状态转变：

- 被转换成Terminated状态，比如调用 `stop()` 方法。
- 被转换成Blocked状态，比如调用了`sleep()`, `wait()` 方法被加入 waitSet 中。
- 被转换成Blocked状态，如进行 IO 阻塞操作，如查询数据库进入阻塞状态。
- 被转换成Blocked状态，比如获取某个锁的释放，而被加入该锁的阻塞队列中。
- 该线程的时间片用完，CPU 再次调度，进入Runnable状态。
- 线程主动调用 yield 方法，让出 CPU 资源，进入Runnable状态。

![Java线程的运行状态转变——码艺术](threadLifeCycle_running2status.png)

## 阻塞状态

Blocked状态的线程能够发生如下状态改变

![Java线程的阻塞状态转变——码艺术](threadLifeCycle_blocked2status.png)

- 被转换成Terminated状态，比如调用 `stop()` 方法，或者是 JVM 意外 Crash。
- 被转换成Runnable状态，阻塞时间结束，如：读取到了数据库数据后。
- 完成了指定时间的休眠，进入到Runnable状态。
- 正在wait中的线程，被其他线程调用`notify()`、`notifyAll()`方法唤醒，进入到Runnable状态。
- 线程获取到了想要的锁资源，进入Runnable状态。
- 线程在阻塞状态下被打断，如其他线程调用了`interrupt()`方法，进入到Runnable状态。

## 终止状态

一旦线程进入了Terminated状态，就意味着这个线程生命的终结，哪些情况下，线程会进入到Terminated状态呢？

- 线程正常运行结束，生命周期结束。
- 线程运行过程中出现意外错误。
- JVM 异常结束，所有的线程生命周期均被结束。



## synchronized使用详解

synchronized关键字





















