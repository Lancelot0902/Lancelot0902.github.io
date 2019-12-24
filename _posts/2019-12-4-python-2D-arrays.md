---
title: python 2D array
date: 2019-12-4 16:30:00 +0800
categories: [python philosophy, 2D array]
tags: [2D array, python]
seo:
  date_modified: 2019-12-24 21:02:59 +0800
---

最近在写python程序时遇到了一个问题，困扰了好几天

代码需要一个二维数组，python中的二维数组有两种声明方式，先讲第一种
```
dp = [[False]*3]*2

>>> 
[
    [false,false,false],
    [false,false,false]
]
```
我们得到一个2行3列的数组，当我们改变某一个值的时候
```
dp[0][0] = True

>>>
[
    [true,false,false],
    [true,false,false]
]
```
发现dp[0][0]和dp[1][0]都发生了改变，这令我百思不得其解，后来查阅得知我们只是创建了2个指向列表的引用，这是什么意思呢？看一下代码的执行顺序

```
dp = [[False]*3]*2

[False]*3
>>> [false，false，false]

[false，false，false]*2

>>> [false，false，false]
    [false，false，false]

```
看着没什么问题，但实际上当我们执行 *2 的时候我们只是创建了两个指向 [false，false，false] 的引用，两个引用指向的是同一内存，所以修改其中的一个另一个肯定也会变

究其根本原因是浅拷贝造成的，用下一种方法就不会有这种情况了

```
dp = [[False for x in range(3)] for y in range(2)]
```
