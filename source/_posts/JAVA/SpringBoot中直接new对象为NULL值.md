---
layout: post
title: "SpringBoot中直接new对象为NULL值"
date: 2019-4-29 16:14:55
comments: true
reward: true
tags: 
	- JAVA
---
#### SpringBoot中直接new对象为NULL值

在springBoot中如果直接new一个对象出来，那么在此对象使用了@Autowired注解都会为NULL值。

![](http://ww1.sinaimg.cn/large/aacc02d8ly1g2ic3hf2v3j215g0le40u.jpg)

为了解决这个问题，我们就要了解Spring容器机制了。

@Autowired是适用于Spring容器的，直接new出来的对象不在Spring容器中，所以@Autowired失效了。

在程序启动的过程中，Spring就创建了容器。我们可以根据ApplicationContext获取到容器中的所有对象。

![](http://ww1.sinaimg.cn/large/aacc02d8ly1g2icmib810j20ov02ywep.jpg)

#### Spring使用：单独使用Bean容器（Bean管理）。

Bean容器初始化基础：依赖两个包

- org.springframework.beans 中的BeanFactory提供配置结构和基本功能，加载并初始化Bean
- org.springframework.context 中的 ApplicationContext保存了Bean对象并在Spring中被广泛使用

Spring注入是指在启动Spring容器加载bean配置的时候，完成对变量的赋值行为。

![](http://ww1.sinaimg.cn/large/aacc02d8ly1fxv1dg2ewfj20wp0c70ue.jpg)