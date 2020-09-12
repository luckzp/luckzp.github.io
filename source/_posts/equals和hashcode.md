---
title: equals和hashcode
urlname: vr81hs
date: 2020-09-10 10:58:55 +0800
tags: [Java]
categories: []
---

equals 方法注重 **两个对象在逻辑上是否相等**。

> 1） 只要重写 equals ，就必须重写 hashCode 。
> 2） 因为 Set 存储的是不重复的对象，依据 hashCode 和 equals 进行判断，所以 Set 存储的
> 对象必须重写这两个方法。
> 3） 如果自定义对象做为 Map 的键，那么必须重写 hashCode 和 equals 。
> 说明： String 重写了 hashCode 和 equals 方法，所以我们可以非常愉快地使用 String 对象
> 作为 key 来使用。
