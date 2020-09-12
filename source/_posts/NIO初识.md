---
title: NIO初识
urlname: gsfglu
date: 2020-09-12 12:19:14 +0800
tags: []
categories: []
---

---

## title: NIO 初识 date: 2020-05-06 19:16:59

tags: Java

### 1.NIO 概念

同步非阻塞 IO(socket 主要的读、写、注册和接收函数，在等待就绪阶段都是非阻塞的，真正的 I/O 操作是同步阻塞的)，通常用于网络服务器进行多路复用 IO。Redis，Nginx, Tomcat8.X 采用这种方式，RabbitMQ 采用类似这种思想。

![](http://ww1.sinaimg.cn/large/aacc02d8gy1gdqtd66t03j20gb05f0tw.jpg#alt=IO%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8.png)

以 nginx 为例，ngnix 会有很多请求进来， epoll 会把他们都监视起来，然后像拨开关一样，谁有数据就拨向谁，然后调用相应的代码处理。

### 2.epoll

epoll 可以理解为 event poll，不同于忙轮询和无差别轮询，epoll 之会把哪个流发生了怎样的 I/O 事件通知我们。此时我们对这些流的操作都是有意义的。（复杂度降低到了 O(k)，k 为产生 I/O 事件的流的个数)。

### 3.selector

新事件(读就绪、写就绪、有新连接到来)到来的时候，会在 selector 上注册标记位，标示可读、可写或者有连接到来。

**Selector.select()**是阻塞的，通过操作系统的通知（epoll）这个函数是阻塞的。可以在一个 while(true)里面调用这个函数而不用担心 CPU 空转。
