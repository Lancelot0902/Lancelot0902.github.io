---
title: CPU Dispatch
date: 2020-1-2 18:30:00 +0800
categories: [operating system, Dispatch]
tags: [operating system]
---

CPU调度是多道程序操作系统的基础，对于支持线程的系统，是内核级线程被调度

### CPU I/O区间周期

进程执行由CPU执行和I/O等待周期组成，进程在这两个状态间切换，直到CPU区间请求终止执行；I/O约束程序通常由许多短的CPU区间组成，CPU约束程序可能有少量长的CPU区间

### CPU调度程序

每当CPU空闲时，操作系统必须从就绪队列选择一个进程来执行，进程选择由短期调度程序或CPU调度程序来执行

### 调度时机

CPU调度决策会在下列四种情况发生：

* 当一个进程从运行态切换到阻塞态（I/O请求或等待事件发生）
* 一个进程从运行态切换到就绪态（中断）
* 一个进程从阻塞态切换到就绪态
* 进程终止

### 调度算法

#### 先到先服务调度（FCFS）

先进先出的调度算法，代码容易理解实现，但平均等待时间较长，其它进程可能会等待一个大的CPU约束进程释放CPU（护航效果），可能导致CPU和设备的使用率较低；允许一个进程长时间占有CPU是个严重错误

FCFS算法是非抢占的，一个进程会控制着CPU资源直到结束

#### 优先级调度算法（PSA）

该算法下的每一个进程都会与一个优先级关联，具有最高优先级的进程会被分配CPU，具有相同优先级的按FCFS调度

该算法的主要问题是无穷阻塞，使某个低优先级的进程无限等待CPU，解决办法之一是老化，逐渐增加等待很长时间的进程的优先级

PSA算法可以是抢占的也可以是非抢占的

#### 最短作业优先调度（SJF）

是优先级调度的一个特例（以CPU区间长度作为优先级），SJF可证明为最佳的，它的平均等待时间最小

SJF算法困难的地方在于如何知道下一CPU区间的时间，SJF常用于长期调度

SJF算法可以是抢占的也可以是非抢占的

#### 轮转法调度（RR）

专门为分时系统设计的，类似于FCFS，但增加的抢占以切换进程，定义一个较小的时间单元为时间片，为每个进程分配不超过一个时间片的时间；轮流为每个进程服务

RR算法的性能很大程度上依赖于时间片的大小，如果时间片很长就变成了FCFS算法，如果很小，那么会浪费时间在上下文切换上

RR算法是抢占的

#### 多级队列反馈调度

组成多个调度队列，每个调度队列的优先级不同，每个调度队列的调度算法也不同，新来的进程先放在优先级最高的末尾，如果该进程CPU区间较长，则将该进程移动到低优先级的队列中；若有进程发生饥饿则执行老化

多级队列反馈调度是最常用的算法，也是最复杂的算法

### 多处理器调度

* 非对称多处理 ：让一个处理器处理所有调度决定，其余处理器负责执行

* 对称多处理（SMP） ：每个处理器自我调度，所有进程可能处于一个共同的就绪队列，也可能处于每个处理器的私有队列

#### 处理器亲和性

由于使缓存无效或重构的代价太高，绝大多数SMP都在避免将一个处理器移动到另一个处理器

#### 负载平衡

设法将工作平均地分配到SMP系统的所有处理器上，通常会打破处理器的亲和性

#### 对称多线程

通过提供几个逻辑处理器来实现同时运行几个线程，在同一个物理处理器上生成多个逻辑处理器，SMT是硬件提供的