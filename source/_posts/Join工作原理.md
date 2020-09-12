---
title: Join工作原理
urlname: nrtt08
date: 2020-09-12 12:18:54 +0800
tags: []
categories: []
---

---

## title: Join 工作原理 date: 2020-05-06 23:22:26

tags: Mysql

### Join 工作原理

### 1.Index Nested-Loop Join

可以使用被驱动表的索引走的是 Index Nested-Loop Join。

执行流程是这样的：

![](http://ww1.sinaimg.cn/large/aacc02d8ly1gej4hmbzpbj20kg0pbn0f.jpg#alt=TIM%E6%88%AA%E5%9B%BE20191127174928.png)

在这个流程里：

1. 对驱动表 t1 做了全表扫描，这个过程需要扫描 100 行；
2. 而对于每一行 R，根据 a 字段去表 t2 查找，走的是树搜索过程。由于我们构造的数据

都是一一对应的，因此每次的搜索过程都只扫描一行，也是总共扫描 100 行； 3. 所以，整个执行流程，总扫描行数是 200。

### 2.Block Nested-Loop Join

不能使用被驱动表的索引,流程是这样的:

![](http://ww1.sinaimg.cn/large/aacc02d8ly1gej4vnonkcj20od0ov77r.jpg#alt=%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20200506230957.png)

在这个流程里：

由于 join_buffer 是以无序数组的方式组织的，因此对表 t2 中的每一行，都要做 100 次判断，总共需要在内存中做的判断次数是：100\*1000=10 万次。

join_buffer 的大小是由参数 join_buffer_size 设定的，默认值是 256k。如果放不下表 t1 的所有数据话，策略很简单，就是分段放。
