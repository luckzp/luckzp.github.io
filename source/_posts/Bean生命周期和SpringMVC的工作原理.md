---
title: Bean生命周期和SpringMVC的工作原理
date: 2020-01-27 15:27:08
tags: Java
---
**Spring Bean生命周期**

![SpringBean生命周期.png](http://ww1.sinaimg.cn/large/aacc02d8gy1gbb5fetvg1j20ui0i1dgz.jpg)

<!--more--> 

![实例化.png](http://ww1.sinaimg.cn/mw690/aacc02d8gy1gba3ri4qgpj20kf0f2tab.jpg)



![初始化.png](http://ww1.sinaimg.cn/large/aacc02d8gy1gbb536clp7j21dx0nyn1m.jpg)

**SpringMVC工作原理**

在SpringBoot启动过程中，创建了Tomcat和DispatcherServlet。

![微信图片_20200127150113.png](http://ww1.sinaimg.cn/large/aacc02d8gy1gbb51vdrccj21cr0jxdil.jpg)

当有请求时，会被DispatcherServlet#doDispatch处理，处理流程图如下：

![MVC流程图.png](http://ww1.sinaimg.cn/large/aacc02d8gy1gbb5f0ds12j20yh0dnwfs.jpg)

[实践源码](https://github.com/luckzp/SpringCode)

