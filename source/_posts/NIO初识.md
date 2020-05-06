---
title: NIO初识
date: 2020-05-06 19:16:59
tags: Java
---
### 1.NIO概念

同步非阻塞IO(socket主要的读、写、注册和接收函数，在等待就绪阶段都是非阻塞的，真正的I/O操作是同步阻塞的)，通常用于网络服务器进行多路复用IO。Redis，Nginx, Tomcat8.X采用这种方式，RabbitMQ采用类似这种思想。

![IO多路复用.png](http://ww1.sinaimg.cn/large/aacc02d8gy1gdqtd66t03j20gb05f0tw.jpg)

以nginx为例，ngnix会有很多请求进来， epoll会把他们都监视起来，然后像拨开关一样，谁有数据就拨向谁，然后调用相应的代码处理。

### 2.epoll

epoll可以理解为event poll，不同于忙轮询和无差别轮询，epoll之会把哪个流发生了怎样的I/O事件通知我们。此时我们对这些流的操作都是有意义的。（复杂度降低到了O(k)，k为产生I/O事件的流的个数)。

### 3.selector 

新事件(读就绪、写就绪、有新连接到来)到来的时候，会在selector上注册标记位，标示可读、可写或者有连接到来。

select是阻塞的，通过操作系统的通知（epoll）这个函数是阻塞的。可以在一个while(true)里面调用这个函数而不用担心CPU空转。