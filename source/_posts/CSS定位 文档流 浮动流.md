---
title: CSS定位 文档流 浮动流
urlname: hbaoci
date: 2020-09-12 12:17:37 +0800
tags: []
categories: []
---

---

## layout: posttitle: "CSS 定位 文档流 浮动"

date: 2017-11-19 19:58
comments: true
reward: true
tags:

- 前端

# ![](http://ww1.sinaimg.cn/large/aacc02d8ly1fxv1ita8utj20n7028aa0.jpg#alt=)

昨天写 CSS 的时候，发现 div 的高度不能自适应。然后在[CSS 浮动](http://www.cnblogs.com/jiqing9006/archive/2012/07/30/2615231.html)找到解决方案了。

普通流就是正常的文档流，在 HTML 里面的写法就是从上到下，从左到右的排版布局。
例：

很显然这是最普通的文档流，从左到右，一个挨一个按照顺序 01 先，02 其次，03 最后排列。

![](http://pic002.cnblogs.com/images/2012/422101/2012073014324651.jpg#alt=img)

一旦给其中的某个 DIV 进行 FLOAT 属性或者 absolute 定位（不包括 static/relative，这两个依然保持正常的文档流），则它完全脱离文档流，不占空间。

例：

![](http://pic002.cnblogs.com/images/2012/422101/2012073014343616.jpg#alt=img)

为了能更好辨认，我分别给 01 绿色，02 灰色，03 黄色。然后再给 01 左浮动。结果，01 脱离了文档流，完全不占空间，所以 02 顺势顶替了 01 原来的位置，结果 02 被 01 盖住了。

同理，absolute 定位跟 FLOAT 一样，脱离了文档流，不再占原来文档流的空间了。再举一个大家在日常经常遇到的问题来印证—高度自适应

反复想一想，高度自适应的原理其实就是这个：

```
<div id=”a”>

<div id=”b”>这是b</div>

<div id=”c”>这是c</div>

</div>
```

这个结构是 a 包住 b 和 c，颜色不变，a 的高度为自动，b 的高度为 100，C 的高度为 500。b 和 c 都为左浮动。

![](http://pic002.cnblogs.com/images/2012/422101/2012073014362919.jpg#alt=img)

很明显 a 没有被撑开了。原因是它们浮动了就不再占空间了。既然没有空间可占，那就等于容器里没有东西，所以不撑开。

知道这个问题后，我就没有将 b 设置为浮动，高度就自适应到 b 的高度了。

![](http://ww1.sinaimg.cn/large/aacc02d8ly1fxv1j866vyj20n7028q2w.jpg#alt=)
