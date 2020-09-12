---
title: Bean生命周期和SpringMVC的工作原理
urlname: gsec0x
date: 2020-09-12 12:18:44 +0800
tags: [Spring]
categories: []
---

**Spring Bean 生命周期**

![](http://ww1.sinaimg.cn/large/aacc02d8gy1gbb5fetvg1j20ui0i1dgz.jpg#align=left&display=inline&height=649&margin=%5Bobject%20Object%5D&originHeight=649&originWidth=1098&status=done&style=none&width=1098)
![](http://ww1.sinaimg.cn/mw690/aacc02d8gy1gba3ri4qgpj20kf0f2tab.jpg#align=left&display=inline&height=509&margin=%5Bobject%20Object%5D&originHeight=509&originWidth=690&status=done&style=none&width=690)

![](http://ww1.sinaimg.cn/large/aacc02d8gy1gbb536clp7j21dx0nyn1m.jpg#align=left&display=inline&height=862&margin=%5Bobject%20Object%5D&originHeight=862&originWidth=1797&status=done&style=none&width=1797)

**SpringMVC 工作原理**

在 SpringBoot 启动过程中，创建了 Tomcat 和 DispatcherServlet。

![](http://ww1.sinaimg.cn/large/aacc02d8gy1gbb51vdrccj21cr0jxdil.jpg#align=left&display=inline&height=717&margin=%5Bobject%20Object%5D&originHeight=717&originWidth=1755&status=done&style=none&width=1755)

当有请求时，会被 DispatcherServlet#doDispatch 处理，处理流程图如下：

![](http://ww1.sinaimg.cn/large/aacc02d8gy1gbb5f0ds12j20yh0dnwfs.jpg#alt=MVC%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

[实践源码](https://github.com/luckzp/SpringCode)
