---
title: Latch
date: 2019-12-19 19:00:00 +0800
categories: [computer science, memory]
tags: [computer science, memory]
---

### 引言
我们都知道CPU运行时计算的数据都是从内存中拿取的（准确的说是寄存器），那么内存的这些信息是如何保存的呢？

### 存储1位
我们先来看如何存储1位数据，也就是 1 和 0

我们知道计算机内的数据传输是以电信号的方式，也就是在电路上；通过设计各种逻辑电路可以得到不同的信号，我们就可以通过设计逻辑电路来永久存储 1 和 0

这是存储1的电路，无论A是什么输出一直都是1

![](https://img-blog.csdnimg.cn/20191219221518678.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xhbmNlbG90MDkwMg==,size_16,color_FFFFFF,t_70)

这是存储0的电路

![](https://img-blog.csdnimg.cn/20191221095940899.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xhbmNlbG90MDkwMg==,size_16,color_FFFFFF,t_70)

有了这两个电路，将它们结合起来就可以做出有用的存储电路

### AND-OR锁存器

![](https://img-blog.csdnimg.cn/20191221100126472.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xhbmNlbG90MDkwMg==,size_16,color_FFFFFF,t_70)

有了锁存器就可以存储1位的信息了，SET输出1，RESET输出0

### 门锁

在锁存器的基础上在加上一些功能就变成了更容易使用的门锁，只有允许写入先为1时才允许写入数据

![](https://img-blog.csdnimg.cn/20191221100442888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xhbmNlbG90MDkwMg==,size_16,color_FFFFFF,t_70)


我们并排放8个锁存器就可以存储8位的信息，一组这样的锁存器就是 **寄存器**

但是现在的计算机都是32位甚至64位，一个32位寄存器就要用32根数据线，32根输出线，再加上1根允许写入线，这都要65根了，更别说64位寄存器了

### 门锁矩阵
使用门锁矩阵可以解决上述问题，我们用16 X 16 网格的锁存器，当要启用某个锁存器时，就打开相应的行和列
