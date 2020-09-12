---
title: JVM内存分区
urlname: ek21d9
date: 2020-08-27 23:35:00 +0800
tags: [JVM]
categories: []
---

####

![](https://cdn.nlark.com/yuque/0/2020/jpeg/431489/1598598539347-502953da-34aa-4f5a-b0b5-68a060247a8b.jpeg#align=left&display=inline&height=409&margin=%5Bobject%20Object%5D&originHeight=491&originWidth=715&size=0&status=done&style=none&width=596)

**线程私有：**

虚拟机栈：线程在执行方法的时候会创建一个栈帧，栈帧包含：局部变量表，操作数栈，动态链接（与其他方法相链接），方法出口等信息。

本地方法栈：与栈类型，不同点是执行 native 方法。

程序计数器：保存当前字节码的位置

**线程共享：**

堆：由垃圾回收器管理。

-Xms: 堆的最小值

-Xmx: 堆的最大值

新生代：Eden:From:To = 8:1:1

方法区：用以存储加载类的信息，常量，静态变量。**JDK8 以前，方法区是在堆永久代中，JDK8 及以后取消了永久代，方法区挪到直接内存 MetaSpace 中。**

永久代：

jdk1.7 及以前： -XX:PermSize -XX:MaxPermSize

jdk1.8 以后：-XX:MetaspaceSize -XX:MaxMetaspaceSize

局部变量存在虚拟机栈中，常量存在方法区中，成员变量则随着对象一起存在堆中。

Java 堆从 GC 的角度还可以细分为: 新生代( Eden 区 、 From Survivor 区 和 To Survivor 区 )和老年
代。

![](https://cdn.nlark.com/yuque/0/2020/jpeg/431489/1598598194156-7a831645-a9c9-473a-a5a7-24a487dacab7.jpeg#align=left&display=inline&height=203&margin=%5Bobject%20Object%5D&originHeight=203&originWidth=675&size=0&status=done&style=none&width=675)
