---
title: promise
date: 2018-03-19 11:34:04
tags: JavaSript
---

---

在新的项目里，团队协作，我想在ajax的异步上使用promise，结合promise的一些特性和优点，提高项目可读性和容错率。

author：桑景瑞
<!-- more -->

在JavaScript的世界中，所有代码都是单线程执行的。

由于这个“缺陷”，导致JavaScript的所有网络操作，浏览器事件，都必须是异步执行。异步执行可以用回调函数实现：

---