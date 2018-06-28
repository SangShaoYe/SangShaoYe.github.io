---
title: webpack
date: 2018-06-28 11:45:09
tags: webpack
---

webapck配置中chunkname是做什么的？

author：桑景瑞
<!-- more -->

filename应该比较好理解，就是对应于entry里面生成出来的文件名。比如：

{
    entry: {
        "index": "pages/index.jsx"
    },
    output: {
        filename: "[name].min.js",
        chunkFilename: "[name].min.js"
    }
}
生成出来的文件名为index.min.js。

chunkname我的理解是未被列在entry中，却又需要被打包出来的文件命名配置。什么场景需要呢？我们项目就遇到过，在按需加载（异步）模块的时候，这样的文件是没有被列在entry中的，如使用CommonJS的方式异步加载模块：

require.ensure(["modules/tips.jsx"], function(require) {
    var a = require("modules/tips.jsx");
    // ...
}, 'tips');
异步加载的模块是要以文件形式加载哦，所以这时生成的文件名是以chunkname配置的，生成出的文件名就是tips.min.js。

（require.ensure() API的第三个参数是给这个模块命名，否则 chunkFilename: "[name].min.js" 中的 [name] 是一个自动分配的、可读性很差的id，这是我在文档很不起眼的地方2.2K发现的。。。）