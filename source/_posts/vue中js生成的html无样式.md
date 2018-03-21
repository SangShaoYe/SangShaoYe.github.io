---
title: vue中js生成的html无样式
date: 2018-03-19 16:44:09
tags: 前端日常问题
---

vue中js生成的html无样式

author：桑景瑞
<!-- more -->

vue中，模块化开发，相信很多人都会在.vue文件中的style标签加上scoped，为了css不相互冲突，
但是发现，在js生成的html上没有css效果，吧scoped删除就好了，这样的话，就要去找scoped的概念了，发现scoped是使css只在本组件中生效，然而为什么js生成的不生效呢，
因为js渲染的html属于本组件的子组件，不明白？对本人生效，对本人的儿子不生效哦，明白了吧。


解决办法：首先，想使用scoped的特性，所以scoped保留，那么，对于js渲染的html的css单独写一个css文件，完了就import进去，就ok了，但是注意哦，这里的css可能会有冲突哦，所以要谨慎使用节点名称。


【完】