---
title: Virtual Memory
date: 2020-1-9 10:30:00 +0800
categories: [operating system, Memory]
tags: [operating system]
---

之前的各种内存管理策略是为了同时将多个进程放在内存中，而在这之前需要将整个进程放在内存中

虚拟内存技术允许执行进程不必完全在内存中，显著优点就是程序可比物理内存大

虚拟内存技术将用户逻辑内存和物理内存分开，一个应用进程通常被分隔成多个物理内存碎片，还有部分暂时存储在外部磁盘上，这样，系统好像为用户提供了一个比实际大的多的内存

### 按需调页

对于按需调页虚拟内存，只有进程执行需要才载入页，类似于交换的分页系统，只不过交换的是整个进程，而调页程序只对进程的单个页操作

需要一定形式的硬件技术来区分哪些页在内存中，哪些页在磁盘上，页表中的有效无效位可以支持这一判断

当进程尝试调入未在内存中的页时会产生页错误陷阱

### 页置换

按需调页增加了多道程序的程度，提高了CPU的利用率和吞吐量，这样做的一个隐患是过度分配内存，也就是说系统在调入新进程时没有空闲页了

解决办法是页置换，查找当前没有使用的帧，将其内容复制到交换空间，改变页表和帧表，将所需页读入空闲帧，改变页表和帧表，重启新进程

实现按需调页的关键是帧分配算法和页置换算法

### 页置换算法

#### FIFO

先进先出的算法

#### OPT/MIN最优置换

换出未来最不长使用的页（向后看）

#### LRU

为每个页关联上次使用的时间，选择最长时间没有使用的页（向前看）

### 帧分配算法

要求为每个进程分配的帧不能超过可用帧的数量，也不能少于最小数量的帧

* 平均分配
* 比例分配

全局分配 ：允许一个进程从所有帧集合中选择一个置换帧

局部分配 ：仅允许进程从自己分配的帧中进行选择

局部分配会阻塞当前进程；全局分配会增加页错误率，但有更好的吞吐量

### 系统颠簸

如果进程没有足够的页，那么会很快产生页错误，那么它必须置换某个页，但是所有的页都在使用，所以需要频繁的页调度，这种行为成为 **颠簸**

如果一个进程在换页上所用的时间多于执行时间，那么这个进程就在颠簸

颠簸的原因 ：操作系统监视CPU的使用率，如果使用率太低，则会引入新的进程，新的进程需要额外的帧，那么这可能会引起系统颠簸，CPU的使用率又会降低，操作系统会继续增加多到程序处理的程度，以至于系统主要忙于调页

为了减少颠簸，系统必须降低多道处理程度，通过中期调度程序将进程调出内存