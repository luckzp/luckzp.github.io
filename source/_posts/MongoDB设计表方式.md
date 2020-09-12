---
title: MongoDB设计表方式
urlname: ovf1rt
date: 2020-09-11 14:53:01 +0800
tags: []
categories: []
---

1. 内嵌模式
   1-N 的情况，通过数组内嵌模式来形成一个文档。
   N-N 的情况，以前在关系性数据库中通过建立中间表来转成一对多的情况，用了 MongoDB 不用映射表，通过冗余的方式实现 N-N。
   ![](https://cdn.nlark.com/yuque/0/2020/jpeg/431489/1599807209756-71b55165-f643-4202-9e60-552e2902c97f.jpeg#align=left&display=inline&height=262&margin=%5Bobject%20Object%5D&originHeight=262&originWidth=821&size=0&status=done&style=none&width=821)

1. 引用模式
   内嵌后文档大小超过 16MB
   数组长度太大（数万以上）
   内嵌文档或数组元素会频繁修改
