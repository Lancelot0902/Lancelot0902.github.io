---
title: transport layer
date: 2019-12-3 20:30:00 +0800
categories: [computer network, chapter 5]
tags: [transport layer]
---
运输层是整个网络体系结构中的关键层次之一，起到承上启下的作用；运输层为应用层提供通信服务，属于面向通信部分的最高层，用户功能的最底层；
### 运输层概述
网络层提供的是主机到主机的逻辑通信，而**运输层提供的是进程间端到端的逻辑通信**

#### 进程间的通信
首先我们要知道，从网络层来看，通信的两端是主机，但是实际上通信的端点是主机上的进程

然后因为一台上的主机有许多的进程，这些进程肯定是使用同一个运输层协议来通信的，不然在某个时间使用一个协议的进程可以通信，而使用另一个协议的进程就不能通信了，这样方式的通信难处理且效率低下，所以就有了两个概念
* 复用 ：在发送方不同的应用进程都可以使用同一个运输层协议通信
* 分用 ：剥去报文的首部后能把数据交付给正确的应用进程

### 运输层的端口
通过协议端口号，我们就可以将数据交付给正确的进程；只要把传送的报文交付到目的主机某个合适的端口，剩下的工作就由TCP/IP来完成，这种在协议栈层间抽象的协议端口就是**软件端口**（硬件端口是不同硬件设备进行交互的接口）

运输层端口号分为两大类
* 服务器端口号（FTP 21,SMTP 25,HTTP 80,HTTPS 443）
* 客户端端口号

### 运输层的两个主要协议
根据应用进程的不同需求，运输层有两种不同的应用协议
* 无连接的UDP
* 面向连接的TCP

### 用户数据报协议UDP
在传送前不用建立连接，接收方在收到后不用作出回复

特点
* 无连接
* 尽最大努力交付
* 面向报文
* 无拥塞控制
* 支持一对一，一对多的交互通信
* 首部开销小

### 传输控制协议TCP
提供面向连接的服务

特点
* 面向连接
* 每一条连接只能有两个端点
* 提供可靠交付
* 提供全双工通信
* 面向字节流

#### TCP的连接
TCP连接的端点不是IP地址，也不是进程，TCP连接的端点叫做套接字（socket），这是数据的最终流向，也就是说**IP层让数据到达正确的主机，运输层让数据到达正确的进程，TCP让数据到达进程中需要使用数据的那部分代码**
* socket = IP地址 ：端口号

#### TCP的可靠传输
原理依靠两个协议
* 停止等待协议 ：每发送完一个协议就停止发送，直到收到接收方的确认信息
* 连续ARQ协议 ：位于发送窗口的分组可以连续发送，每接收到接收方的一个确认分组就把窗口向前滑动一个位置；接收方采取累积确认的方式，对按序到达的最后一个分组发送确认

实现
* 滑动窗口协议 ：以字节为单位，把应用进程的字节流写入TCP的发送缓存中，接收方的应用进程从TCP的接受缓存中读取字节流

#### TCP的流量控制
控制发送方的发送速率，以免接收方来不及接收；使用滑动窗口协议

#### TCP的拥塞控制
**拥塞** ：在某段时间，对网络中某一资源的需求超过了该资源所能提供的可用部分

**拥塞控制** ：利用拥塞控制算法，防止过多的数据注入到网络中，防止网络中的路由器和链路过载，是网络能承受现有的负荷

TCP拥塞控制算法
* 慢开始
* 拥塞避免
* 快重传
* 快恢复

### TCP的运输连接管理
TCP的连接有三个阶段
* 连接建立
* 数据传送
* 连接释放

#### 三次握手
三次握手发生在连接建立阶段，TCP的连接建立采用客户-服务器方式，主动发起连接的叫客户，被动等待连接的叫服务器

![](https://img-blog.csdnimg.cn/20191204092739759.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xhbmNlbG90MDkwMg==,size_16,color_FFFFFF,t_70)

为什么要建立三次握手呢？

首先前两次的握手是非常明确的，第一次握手客户端发送建立连接的请求，第二次握手服务器发送连接建立的响应。

到此就可以建立连接了，那么为什么还需要第三次握手呢，书上给的解释是防止已经失效的连接请求报文突然又传到服务端，因而产生错误。

举个例子，假如只有两次握手，而且第一个握手请求在互联网中迷路了，然后客户机超时重传了第二个握手请求，第二个请求到达服务器，在收到服务器的响应后开始通信，通信完毕后关闭连接，一切都正常进行，但是连接关闭后第一个握手请求到达了服务器，虽然迷路了但还是到了，服务器收到后发送响应报文然后开始等待接收数据，但是客户机的数据已经发送完了不会再发送了，所以服务器只是白白等着，浪费了资源

所以需要第三次握手，客户机再响应一下，就可以避免这种情况

#### 四次握手
四次握手发生在连接释放阶段

![](https://img-blog.csdnimg.cn/20191204093916601.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xhbmNlbG90MDkwMg==,size_16,color_FFFFFF,t_70)

* 第一次挥手 ：告诉服务器想要关闭连接
* 第二次挥手 ：服务器告诉客户机已收到消息，但要等我这里的数据接收完毕后
* 第三次挥手 ：告诉客户机，我这里的数据接收完毕，可以关闭了
* 第四次挥手 ：发送一个确认报文告诉服务器要关闭

确认报文发送后要等待一个时间2MSL，保证服务器收到ACK确认报文，防止ACK的丢失而使服务器白白等待
