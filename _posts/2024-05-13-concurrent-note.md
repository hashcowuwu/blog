---
layout: post
title: 并发学习笔记 
date: 2023-05-13
Author: Shengbin 
tags: [algorithm]
comments: true
toc: true
---

>Every Computer System Is a State Machine


![](https://upload.wikimedia.org/wikipedia/commons/e/ea/AmdahlsLaw.svg)



## 并发概念的理解

如过我们将串行程序执行看作一个状态机，那么不妨将并发程序看作一个有向无环图。

状态机的增多导致了状态数的指数呈长，几乎无法通过检查状态的方式来保证程序的正确性


>想象一下两个线程同时执行这个函数

```cpp
unsigned int balance = 100;
int* T(void* amt) {
    if (balance >= amt) {
        balance -= amt;
        return true;
    } else {
        return false;
    }
}

```

![](https://blog.bubble.ro/wp-content/uploads/2014/12/multithreaded-programming.jpg)

并发可以被理解为多个状态机同时运行，每个状态机代表一个独立的计算或任务。这些状态机可以并行地执行，即它们的状态转换可以同时发生，而不是按照严格的顺序进行。

* 独立性：每个状态机独立运行，有自己的状态和转换逻辑。
* 交错执行：状态机的状态转换可以交错发生，形成并发执行的效果。
* 同步需求：状态机之间可能需要同步，以确保数据的一致性和避免竞态条件。
* 资源共享：状态机可能需要共享资源，如内存、文件等，这需要适当的资源管理策略。

并发的难度在于**CPU乱序执行，编译器优化，宽松内存模型**，并发控制则是确保不同计算执行之间的交互或通信的正确顺序，并协调对执行之间共享的资源的访问。




错误的并发会造成数据竞争(Data Race)，解决并发的最好的方法就是不并发,同步原语通过限制部分并发来实现对程序的协调。通过同步原语来实现并发控制。

## 同步原语 🍏

### 互斥(Mutual exclusion)🔒

当我们在某个时刻不想并发时.通过互斥来实现在线程切换前时完成操作,保障在时间段内只有线程对资源的控制。互斥锁保证的区域通常被称作临界区此类问题通常被称做临界区问题。,互斥锁的实现单核系统上的处理通常是用关中断来实现或自旋。多核上则是通过硬件提供的原子操作设计的软件算法解决。只有正确使用锁才能实现正确的实现并发的处理。

#### 实现🌪

应用层面自旋锁是由软件算法与操作系统所提供的api一起实现的,内核层要正确实现互斥需要关中断(保证操作的原子性避免死锁)+自旋。上锁解锁前后状态不变,保存状态

### 条件变量

条件变量（Condition Variable）是多线程编程中用于同步的一种机制，它允许一个或多个线程等待某个特定条件的发生，直到其他线程通知这个条件已经满足。条件变量通常与互斥锁（Mutex）一起使用，以确保在检查和修改共享状态时的互斥性。


### 信号量






## 并发bug










# 参考

>Wikipedia  https://en.wikipedia.org/wiki/Concurrency_(computer_science)

>wikipedia https://en.wikipedia.org/wiki/Memory_model_(programming)  

>jyy老师 jyywiki https://jyywiki.cn/OS/2024/lect8.md

>陈海波《操作系统原理和实现》

