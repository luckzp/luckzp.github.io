---
title: Spring学习笔记
urlname: rdhuz4
date: 2020-08-28 15:15:00 +0800
tags: [Spring]
categories: []
---

在面试中，经常会问，说说你对 spring IOC 和 AOP 的理解，问题很宽泛，似乎不知道从何说起。
回答思路：1.先用通俗易懂的话解释下何为 IOC 和 AOP---------》2.各自的实现原理-----------》3.自己的项目中如何使用

## IOC

#### 一.定义

IOC 是反转控制 (Inversion Of Control)的缩写，就像控制权从本来在自己手里，交给了 Spring。
Spring IOC 的作用

1. 不必自己创建对象了（不必 new 出来了）
1. 面向接口编程

许多应用都是通过彼此间的相互合作来实现业务逻辑的，如类 A 要调用类 B 的方法，以前我们都是在类 A 中，通过自身 new 一个类 B，然后在调用类 B 的方法，这样对象间的耦合度高了，现在我们把 new 类 B 的事情交给 spring 来做，在我们调用的时候，容器会为我们实例化。

#### 二.依赖注入（DI）

资源定位，即定义 bean 的 xml（也可以使用@Bean）-------》载入--------》IOC 容器注册，注册 beanDefinition。
IOC 容器的初始化过程，一般不包含 bean 的依赖注入的实现，在 spring IOC 设计中，bean 的注册和依赖注入是两个过程，，依赖注入一般发生在应用第一次索取 bean 的时候，但是也可以在 xm 中配置，在容器初始化的时候，这个 bean 就完成了初始化。
Spring 使用：单独使用 Bean 容器（Bean 管理）。
Bean 容器初始化基础：依赖两个包

- org.springframework.beans 中的 BeanFactory 提供配置结构和基本功能，加载并初始化 Bean
- org.springframework.context 中的 ApplicationContext 保存了 Bean 对象并在 Spring 中被广泛使用

Spring 注入是指在启动 Spring 容器加载 bean 配置的时候，完成对变量的赋值行为。

![](http://ww1.sinaimg.cn/large/aacc02d8ly1fxv1dg2ewfj20wp0c70ue.jpg#align=left&display=inline&height=439&margin=%5Bobject%20Object%5D&originHeight=439&originWidth=1177&status=done&style=none&width=1177)

## AOP

面向切面，所有业务都要处理的业务，如打印日志，登录拦截。要记录所有 update\*方法的执行时间时间，操作人等等信息，记录到日志，

通过 spring 的 AOP 技术，就可以在不修改 update\*的代码的情况下完成该需求。

spring 用代理类包裹切面，把他们织入到 Spring 管理的 bean 中。也就是说代理类伪装成目标类，它会截取对目标类中方法的调用，让调用者对目标类的调用都先变成调用伪装类，伪装类中就先执行了切面，再把调用转发给真正的目标 bean。

前一种实现 InvocationHandler 的 invoke 方法，spring 会使用 JDK 的 java.lang.reflect.Proxy 类，它允许 Spring 动态生成一个新类来实现必要的接口，织入通知，并且把对这些接口的任何调用都转发到目标类。

后一种父子模式，spring 使用 CGLIB 库生成目标类的一个子类，在创建这个子类的时候，spring 织入通知，并且把对这个子类的调用委托到目标类。

Spring AOP 会动态选择使用 JDK 动态代理、CGLIB 来生成 AOP 代理，如果目标类实现了接口，Spring AOP 则无需 CGLIB 的支持，直接使用 JDK 提供的 Proxy 和 InvocationHandler 来生成 AOP 代理即可。

![](http://ww1.sinaimg.cn/large/aacc02d8ly1fxv1dpe89mj20ns0eajug.jpg#align=left&display=inline&height=514&margin=%5Bobject%20Object%5D&originHeight=514&originWidth=856&status=done&style=none&width=856)

### JDBC 数据库全过程

1. 加载驱动
2. 创建数据库链接
3. 创建 Statement 对象
4. **执行 SQL 获取数据（MyBatis 关注这里）**
5. 数据转化
6. 资源释放
