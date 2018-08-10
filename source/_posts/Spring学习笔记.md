---
layout: post
title: "Spring学习笔记"
comments: true
reward: true
tags: 
	- JAVA
---
抽象类：不可以多重继承。

接口：可以多重继承。

<!--more--> 

## IOC

IOC是反转控制 (Inversion Of Control)的缩写，就像控制权从本来在自己手里，交给了Spring。 

Spring IOC的作用

1. 不必自己创建对象了（不必new出来了）
2. 面向接口编程

Spring使用：单独使用Bean容器（Bean管理）。

Bean容器初始化基础：依赖两个包

- org.springframework.beans 中的BeanFactory提供配置结构和基本功能，加载并初始化Bean
- org.springframework.context 中的 ApplicationContext保存了Bean对象并在Spring中被广泛使用

Spring注入是指在启动Spring容器加载bean配置的时候，完成对变量的赋值行为。

![](http://ovuyz1070.bkt.clouddn.com/18-3-23/94560729.jpg)

## AOP

面向切面，所有业务都要处理的业务，如打印日志，登录拦截。

![](http://ovuyz1070.bkt.clouddn.com/18-3-23/29890993.jpg)

### JDBC数据库全过程

1. 加载驱动
2. 创建数据库链接
3. 创建Statement对象
4. **执行SQL获取数据（MyBatis关注这里）**
5. 数据转化
6. 资源释放