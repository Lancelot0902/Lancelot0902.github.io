---
title: python memory allocation
date: 2019-12-2 19:30:00 +0800
categories: [python philosophy, memory allocation]
tags: [memory allocation, python]
seo:
  date_modified: 2019-12-03 19:24:27 +0800
---
讲一讲在python中对变量的内存分配

首先看一下c/c++语言中执行
```
int a = 1;
cout << &a;

>>> 0x73fe1c
```
代表了在内存中找出一块内存来分给变量a，这块内存的值为1，再执行
```
a = 2;
cout << &a;

>>> 0x73fe1c
```
可以看出内存地址没有变化，也就是说变量a和这块内存绑定了，改变a的值只是改变这块内存的值

再来看看Python中的变量分配
```
a = 1
print(id(a))

>>> 140724140667136

a = 2
print(id(a))

>>> 140724140667168
```
发现两次变量a的地址不一样了，这是为什么呢？

因为在Python中一切皆是对象，而变量只是对变量的引用 ( 或称标签 )，对对象的操作都是通过对引用来完成的，所以数字 1 是一个对象，内存为这个对象分配空间，而a只是一个标签，在执行 a=2后，这个标签绑定了另外一个对象，也就是2，用图来表示

![](https://img-blog.csdnimg.cn/20191202220917450.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xhbmNlbG90MDkwMg==,size_16,color_FFFFFF,t_70)

变量只是一个标签，本身并不具有任何信息，而类型信息存储在对象中，这和c/c++有很大的出入
