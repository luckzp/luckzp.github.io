---
title: Bean生命周期和SpringMVC的工作原理
urlname: gsec0x
date: 2020-09-12 12:18:44 +0800
tags: []
categories: []
---

---

## title: Bean 生命周期和 SpringMVC 的工作原理 date: 2020-01-27 15:27:08

tags: Java

**Spring Bean 生命周期**

![](http://ww1.sinaimg.cn/large/aacc02d8gy1gbb5fetvg1j20ui0i1dgz.jpg#alt=SpringBean%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)
![](http://ww1.sinaimg.cn/mw690/aacc02d8gy1gba3ri4qgpj20kf0f2tab.jpg#alt=%E5%AE%9E%E4%BE%8B%E5%8C%96.png)

![](http://ww1.sinaimg.cn/large/aacc02d8gy1gbb536clp7j21dx0nyn1m.jpg#alt=%E5%88%9D%E5%A7%8B%E5%8C%96.png)

**SpringMVC 工作原理**

在 SpringBoot 启动过程中，创建了 Tomcat 和 DispatcherServlet。

![](http://ww1.sinaimg.cn/large/aacc02d8gy1gbb51vdrccj21cr0jxdil.jpg#alt=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20200127150113.png)

当有请求时，会被 DispatcherServlet#doDispatch 处理，处理流程图如下：

![](http://ww1.sinaimg.cn/large/aacc02d8gy1gbb5f0ds12j20yh0dnwfs.jpg#alt=MVC%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

[实践源码](https://github.com/luckzp/SpringCode)
