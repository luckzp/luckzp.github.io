---
layout: post
title: "CSS定位 文档流 浮动"
date: 2017-11-19 19:58
comments: true
reward: true
tags: 
	- 前端
---
# ![](http://ww1.sinaimg.cn/large/aacc02d8ly1fxv1ita8utj20n7028aa0.jpg)

昨天写CSS的时候，发现div的高度不能自适应。然后在[CSS 浮动](http://www.cnblogs.com/jiqing9006/archive/2012/07/30/2615231.html)找到解决方案了。

普通流就是正常的文档流，在HTML里面的写法就是从上到下，从左到右的排版布局。

<!--more--> 

例：

<div id=”01”></div><div id=”02”></div><div></div>

很显然这是最普通的文档流，从左到右，一个挨一个按照顺序01先，02其次，03最后排列。

![img](http://pic002.cnblogs.com/images/2012/422101/2012073014324651.jpg)

一旦给其中的某个DIV进行FLOAT属性或者absolute定位（不包括static/relative，这两个依然保持正常的文档流），则它完全脱离文档流，不占空间。

例：

![img](http://pic002.cnblogs.com/images/2012/422101/2012073014343616.jpg)

为了能更好辨认，我分别给01绿色，02灰色，03黄色。然后再给01左浮动。结果，01脱离了文档流，完全不占空间，所以02顺势顶替了01原来的位置，结果02被01盖住了。

同理，absolute定位跟FLOAT一样，脱离了文档流，不再占原来文档流的空间了。再举一个大家在日常经常遇到的问题来印证—高度自适应

反复想一想，高度自适应的原理其实就是这个：

```
<div id=”a”>

<div id=”b”>这是b</div>

<div id=”c”>这是c</div>

</div>
```

这个结构是a包住b和c，颜色不变，a的高度为自动，b的高度为100，C的高度为500。b和c都为左浮动。

![img](http://pic002.cnblogs.com/images/2012/422101/2012073014362919.jpg)

很明显a没有被撑开了。原因是它们浮动了就不再占空间了。既然没有空间可占，那就等于容器里没有东西，所以不撑开。

知道这个问题后，我就没有将b设置为浮动，高度就自适应到b的高度了。

![](http://ww1.sinaimg.cn/large/aacc02d8ly1fxv1j866vyj20n7028q2w.jpg)