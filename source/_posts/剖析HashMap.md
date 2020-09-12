---
title: 剖析HashMap
urlname: rtkp4k
date: 2020-09-10 10:23:27 +0800
tags: [Java]
categories: []
---

1. HashMap 集合的底层原理。

转自：[https://juejin.im/post/5aa5d8d26fb9a028d2079264#heading-23](https://juejin.im/post/5aa5d8d26fb9a028d2079264#heading-23)

![](https://cdn.nlark.com/yuque/0/2020/jpeg/431489/1599706956303-dd02c4eb-4a08-4bb7-8e3f-18904b2d1b22.jpeg#align=left&display=inline&height=719&margin=%5Bobject%20Object%5D&originHeight=719&originWidth=789&size=0&status=done&style=none&width=789)
![](https://cdn.nlark.com/yuque/0/2020/jpeg/431489/1599706956379-770c7607-378e-4c65-947a-4a8d07c24b5b.jpeg#align=left&display=inline&height=1750&margin=%5Bobject%20Object%5D&originHeight=1750&originWidth=1070&size=0&status=done&style=none&width=1070)

### 2. HashMap 为什么线程不安全

- Put 的时候会导致数据会覆盖
- 删除键值对
- addEntry 中当加入新的键值对后键值对总数量超过门限值的时候会调用一个 resize 操作

比如有两个线程 A 和 B，首先 A 希望插入一个 key-value 对到 HashMap 中，首先计算记录所要落到的桶的索引坐标，然后获取到该桶里面的链表头结点，此时线程 A 的时间片用完了，而此时线程 B 被调度得以执行，和线程 A 一样执行，只不过线程 B 成功将记录插到了桶里面，假设线程 A 插入的记录计算出来的桶索引和线程 B 要插入的记录计算出来的桶索引是一样的，那么当线程 B 成功插入之后，线程 A 再次被调度运行时，它依然持有过期的链表头但是它对此一无所知，以至于它认为它应该这样做，如此一来就覆盖了线程 B 插入的记录，这样线程 B 插入的记录就凭空消失了。
