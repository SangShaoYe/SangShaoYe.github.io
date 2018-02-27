---
title: angularjs爬坑
date: 2018-02-23 16:49:15
tags: angularjs
---

//本md有问题最多一千四，一天25,900.

# 如何衡量一个人的angularjs水平？
## 论述的问题：
>1.angular的数据绑定采用什么机制？详述原理

脏数据检查 != 轮询检查更新

谈起angular的脏检查机制(dirty-checking), 常见的误解就是认为： 
ng是定时轮询去检查model是否变更。
其实，ng只有在指定事件触发后，才进入$digest cycle：

- DOM事件，譬如用户输入文本，点击按钮等。(ng-click)
- XHR响应事件 ($http)
- 浏览器Location变更事件 ($location)
- Timer事件($timeout, $interval)
- 执行$digest()或$apply()

有很多方式实现双向绑定，目前AngularJS选择了dirty check。简单来说就是给每个需要绑定的元素加个watcher，缓存下oldValue，然后定时遍历所有的watcher，比较newValue和oldValue，如果变化了做更新操作。

- [ ] **单次绑定  {{::variable}}**

>2.两个平级界面块a和b，如果a中触发一个事件，有哪些方式能让b知道，详述原理

>3.一个angular应用应当如何良好地分层？

>4.angular应用常用哪些路由库，各自的区别是什么？

>5.如果通过angular的directive规划一套全组件化体系，可能遇到哪些挑战？

>6.分属不同团队进行开发的angular应用，如果要做整合，可能会遇到哪些问题，如何解决？

>7.angular的缺点有哪些？

>8.如何看待angular 1.2中引入的controller as 语法？

>9.详述angular的“依赖注入”

>10.如何看待angular 2？
## 简单的问题：
>1.ng-if跟ng-show/hide的区别有哪些？

if是删除或者增加整个代码段，而show、hide是通过css中的display属性的block和none
来对元素进行显示或隐藏，具有本质的区别，其中当初次加载页面的时候就能决定的，并且不会变动的用if好，经常变动的用show和hide更好。

>2.ng-repeat迭代数组的时候，如果数组中有相同值，会有什么问题，如何解决？

如果相同的值，他就报错，解决办法：

1.在ng-report在中，和vue的v-for相似，注意，在数据中是否有重复数据，若有重复数据，可能会抱错，这个时候，要在表达式后面加上 track by $index，
例如：


```
<li ng-repeat="x in names track by $index">
      {{x}}.{{lastname}} 
      <button ng-click="delPerson($index)">删除</button> 
  </li> 
names为数组，$index为下标
$scope.delPerson = function(index){
    // 将点击删除的对象从数组中移除，angular会自动更新列表
    $scope.names.splice(index,1);            
  }
```


>3.ng-click中写的表达式，能使用JS原生对象上的方法，比如Math.max之类的吗？为什么？

不可以，不止是 ng-click 中的表达式，只要是在页面中，都不能直接调用原生的 JS 方法，因为这些并不存在于与页面对应的 Controller 的 $scope 中。
举个栗子：

```
<p>{{parseInt(55.66)}}<p>
```

会发现，什么也没有显示。但如果在 $scope 中添加了这个函数：

```
$scope.parseInt = function(x){
    return parseInt(x);
}
```

这样自然是没什么问题了。对于这种需求，使用一个 filter 或许是不错的选择：

```
<p>{{13.14 | parseIntFilter}}</p>
app.filter('parseIntFilter', function(){
    return function(item){
        return parseInt(item);
    }
})
```


>4.{{now | ‘yyyy-MM-dd’}}这种表达式里面，竖线和后面的参数通过什么方式可以自定义？

>5.factory和service，provider是什么关系？