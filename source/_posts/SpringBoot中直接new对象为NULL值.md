---
title: SpringBoot中直接new对象为NULL值
urlname: kvg8bq
date: 2020-08-28 15:14:55 +0800
tags: [Spring]
categories: []
---

#### SpringBoot 中直接 new 对象为 NULL 值

在 springBoot 中如果直接 new 一个对象出来，那么在此对象使用了@Autowired 注解都会为 NULL 值。

![](http://ww1.sinaimg.cn/large/aacc02d8ly1g2ic3hf2v3j215g0le40u.jpg#align=left&display=inline&height=770&margin=%5Bobject%20Object%5D&originHeight=770&originWidth=1492&status=done&style=none&width=1492)

为了解决这个问题，我们就要了解 Spring 容器机制了。

@Autowired 是适用于 Spring 容器的，直接 new 出来的对象不在 Spring 容器中，所以@Autowired 失效了。

在程序启动的过程中，Spring 就创建了容器。我们可以根据 ApplicationContext 获取到容器中的所有对象。

![](http://ww1.sinaimg.cn/large/aacc02d8ly1g2icmib810j20ov02ywep.jpg#align=left&display=inline&height=106&margin=%5Bobject%20Object%5D&originHeight=106&originWidth=895&status=done&style=none&width=895)
