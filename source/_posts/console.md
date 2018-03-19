---
title: console
date: 2018-03-06 09:15:51
tags: JavaSript
---

关于console调试的一些小方法。

author：桑景瑞
<!-- more -->
---
console 方法
当你想要像大牛一样快速的调试时，console API 是个不错的解决方案。下面是几个能帮你大幅简化调试并节约时间的例子。
输出堆栈信息
我发现最常见的一个问题是人们在追踪事件以及寻找某个函数的调用位置时经常迷路。
我推荐使用 console.trace() 方法！你可以通过这个简单的方法来跟踪你的调用信息，详细的了解它从哪里开始，以及整个生命周期。
<figure name="7856" id="7856"> <canvas width="75" height="47"></canvas> 

<figcaption> 在这个例子中，我添加了一个断点用于在 RangeSliderObject 中插入 console.trace()。当 RangeSlider 按钮被触发后，堆栈信息将会被输出到控制台。 </figcaption> </figure>
像表格数据一样展示对象信息
这是一个个人爱好，即通过 console.table() 来友好的在控制台中显示对象和数组的内容。现在对在控制台中麻烦的通过点击来查看对象内容说拜拜吧。
<figure name="b03c" id="b03c"> <canvas width="75" height="22"></canvas> 

<figcaption> 在控制台中用 console.table() 方法包裹你的对象后，你会得到一个表格化的输出。你也可以在浏览器中加一个断点，用于你想测试的代码，然后通过 console.table() 将结果输出到控制台。 </figcaption> </figure>
设置一个计时器
如果你想知道一段代码的执行会消耗多长时间，你会怎么做？关于这部分我见过一些黑魔法，不过其实解决方案很朴实。你唯一需要做的就是通过 console.time() 来开始一个计时器，然后在需要的时候通过 console.timeEnd() 来结束它。
<figure name="809a" id="809a"> <canvas width="75" height="20"></canvas> 

<figcaption> 在这个例子中，我添加了一个用于测试的 console.time()，在执行了一些代码后再通过 console.timeEnd() 结束它。 </figcaption> </figure>
检查 CPU 使用量
通过 console.profile() 方法可以非常方便的检测 CPU 的状况并且分析每次运行代码所消耗的时间。这可以让你知道在你的 Javascript 代码中哪些代码消耗比较大。
<figure name="7a93" id="7a93"> <canvas width="75" height="40"></canvas> 

<figcaption> 在控制台中，通过 console.profile() 来开始，通过 console.profileEnd() 来结束。然后你就可以在 Profiles 选项卡中查看结果。 </figcaption> </figure>
检查对象属性
如果你需要一个对象的 Javascript 展现方式，你可以使用 console.dir() 方法，它会输出那个对象的属性列表。
<figure name="8473" id="8473"> <canvas width="75" height="45"></canvas> 

<figcaption> 就像 console.trace() 的例子一样，这个例子也都是在控制台中完成的。Stas Gavrylov 还提醒了我别忘了这仅仅适用于 DOM 节点。谢谢！ </figcaption> </figure>
3 个额外的小建议
这里有 3 个关于如何轻松的调试的小建议。
快速的找到想调试的函数
有时候你希望在有很多资源文件的情况下快速的调试一个函数，但使用 Ctrl-Shift + F 的快捷键并不能找到你希望的结果，这时候通过将函数包裹在 debug() 方法中可以让其在被执行的时候暂停程序。
<figure name="b52d" id="b52d"> <canvas width="75" height="66"></canvas> 

<figcaption> 在这个例子中，我在控制台中将 buttonGroup 函数用 debug 方法包装了起来。于是代码在它被调用的时候被暂停了下来  — 在我们的例子中是通过在控制台中直接调用的方式来触发的。 </figcaption> </figure>
监听一个函数的参数
假设你只是单纯的想监听传给某个函数的参数，并且不希望代码被杂乱的输出变得臃肿。那么你可以使用 monitor() 方法来获取到那些珍贵的参数。
<figure name="9dc6" id="9dc6"> <canvas width="75" height="23"></canvas> 

<figcaption> 在这个例子中，我在控制台中将 RangeSliderObject 用 monitor 方法包装了起来。于是当函数被调用的时候，参数情况便会被输出在控制台中。 </figcaption> </figure>
获取 iframe 中控制台的内容
我曾的多次看到有人想获取另一个标签页中 iframe 里的控制台内容。有时候他们需要父页面上下文，因此他们不得不手动操作控制台的一些数据。
来回这样切换是不切实际的，但有一个快速的解决方案。你可以在浏览器的控制台选项卡下拉菜单中，选择显示所有浏览器正在渲染的文档，这样切换起来就轻松多了：
<figure name="7f88" id="7f88"> <canvas width="75" height="32"></canvas> 

 <figcaption> 这部分在 Console 选项卡中也可以完成。 </figcaption> </figure>

---
