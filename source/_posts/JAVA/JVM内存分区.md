---
layout: post
title: "JVM内存分区"
tags: 
	- JAVA
---

局部变量存在虚拟机栈中，常量存在方法区中，成员变量则随着对象一起存在堆中。

虚拟机栈存的是方法，每个方法包括：局部变量表，操作数栈，动态链接（与其他方法相链接），出口。

![](http://ww1.sinaimg.cn/large/aacc02d8ly1fxuyutofxmj20vw0lidrd.jpg)



![](http://ww1.sinaimg.cn/large/aacc02d8ly1fxuyvjd0o1j20qr0fddju.jpg)