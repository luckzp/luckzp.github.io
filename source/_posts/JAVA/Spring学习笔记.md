---
layout: post
title: "Spring学习笔记"
comments: true
data: 2019-4-29 09:48:22
tags: 
	- JAVA
---
在面试中，经常会问，说说你对spring IOC和AOP的理解，问题很宽泛，似乎不知道从何说起。

回答思路：1.先用通俗易懂的话解释下何为IOC和AOP---------》2.各自的实现原理-----------》3.自己的项目中如何使用

<!--more--> 

## IOC

#### 一.定义

IOC是反转控制 (Inversion Of Control)的缩写，就像控制权从本来在自己手里，交给了Spring。 

Spring IOC的作用

1. 不必自己创建对象了（不必new出来了）
2. 面向接口编程

许多应用都是通过彼此间的相互合作来实现业务逻辑的，如类A要调用类B的方法，以前我们都是在类A中，通过自身new一个类B，然后在调用类B的方法，这样对象间的耦合度高了，现在我们把new类B的事情交给spring来做，在我们调用的时候，容器会为我们实例化。

#### 二.依赖注入（DI）

资源定位，即定义bean的xml（也可以使用@Bean）-------》载入--------》IOC容器注册，注册beanDefinition。

IOC容器的初始化过程，一般不包含bean的依赖注入的实现，在spring IOC设计中，bean的注册和依赖注入是两个过程，，依赖注入一般发生在应用第一次索取bean的时候，但是也可以在xm中配置，在容器初始化的时候，这个bean就完成了初始化。

Spring使用：单独使用Bean容器（Bean管理）。

Bean容器初始化基础：依赖两个包

- org.springframework.beans 中的BeanFactory提供配置结构和基本功能，加载并初始化Bean
- org.springframework.context 中的 ApplicationContext保存了Bean对象并在Spring中被广泛使用

Spring注入是指在启动Spring容器加载bean配置的时候，完成对变量的赋值行为。

![](http://ww1.sinaimg.cn/large/aacc02d8ly1fxv1dg2ewfj20wp0c70ue.jpg)

## AOP

面向切面，所有业务都要处理的业务，如打印日志，登录拦截。要记录所有update*方法的执行时间时间，操作人等等信息，记录到日志，

通过spring的AOP技术，就可以在不修改update*的代码的情况下完成该需求。

spring用代理类包裹切面，把他们织入到Spring管理的bean中。也就是说代理类伪装成目标类，它会截取对目标类中方法的调用，让调用者对目标类的调用都先变成调用伪装类，伪装类中就先执行了切面，再把调用转发给真正的目标bean。

前一种兄弟模式，spring会使用JDK的java.lang.reflect.Proxy类，它允许Spring动态生成一个新类来实现必要的接口，织入通知，并且把对这些接口的任何调用都转发到目标类。

后一种父子模式，spring使用CGLIB库生成目标类的一个子类，在创建这个子类的时候，spring织入通知，并且把对这个子类的调用委托到目标类。

Spring AOP 会动态选择使用 JDK 动态代理、CGLIB 来生成 AOP 代理，如果目标类实现了接口，Spring AOP 则无需 CGLIB 的支持，直接使用 JDK 提供的 Proxy 和 InvocationHandler 来生成 AOP 代理即可。

![](http://ww1.sinaimg.cn/large/aacc02d8ly1fxv1dpe89mj20ns0eajug.jpg)

### JDBC数据库全过程

1. 加载驱动
2. 创建数据库链接
3. 创建Statement对象
4. **执行SQL获取数据（MyBatis关注这里）**
5. 数据转化
6. 资源释放