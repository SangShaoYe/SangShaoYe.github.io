---
title: ECMA6
date: 2018-03-06 15:22:43
tags: JavaSript
---

ECMA6的部分新特性的讲解
author：桑景瑞
<!-- more -->

# ECMA6
###### 首先，一直想针对ECMA6搞搞东西，现在就来搞一搞。

##### 搞ECMA6的话首先还是需要有ECMA5的基础，然后来了解这门语言的最新进展ECMA6在2015年6月发布，目标的让JavaScript可以用来编写复杂的大型应用程序，成为企业级开发语言。这句话提现两点：
###### 1.JavaScript的发展，JavaScript将慢慢成为一门适应场景更广泛抗压力更强同时还秉承其轻量，语言相对简单等特点。（多的就不说了，都懂）
###### 2.企业级语言是指那些为商业组织、大型企业而创建并部署的应用所编写使用的语言。这些大型企业级应用的结构复杂，涉及的外部资源众多、事务密集、数据量大、用户数多，有较强的安全性考虑。 所以说，JavaScript既成为企业级语言的话，其各方面无论稳健性，解释性，安全性，可移植性等都会完全符合企业级。现在来说，能称的上企业级的应该也只有JAVA了。	

---

###### 有人可能会问，不是说ECMA6吗，怎么一直在说JavaScript ，所以一个常见的问题就来了，ECMAScript 和 JavaScript 到底是什么关系？
###### 要讲清楚这个问题，需要回顾历史。1996 年 11 月，JavaScript 的创造者 Netscape 公司，决定将 JavaScript 提交给标准化组织 ECMA，希望这种语言能够成为国际标准。次年，ECMA 发布 262 号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言称为 ECMAScript，这个版本就是 1.0 版。该标准从一开始就是针对JavaScript 语言制定的，但是之所以不叫 JavaScript，有两个原因。一是商标，Java 是 Sun 公司的商标，根据授权协议，只有 Netscape 公司可以合法地使用 JavaScript 这个名字，且JavaScript 本身也已经被 Netscape 公司注册为商标。二是想体现这门语言的制定者是ECMA，不是 Netscape，这样有利于保证这门语言的开放性和中立性。因此，ECMAScript 和 JavaScript 的关系是，前者是后者的规格，后者是前者的一种实现（另外的 ECMAScript 方言还有 Jscript 和 ActionScript）。日常场合，这两个词是可以互换的。
###### 在这里JavaScript 和 ECMAScript 的历史就不做陈述了，可以自行百度去了解一下。言归正传。说了这么多，大家也能看出来，本文主要是对ECMA6的部分新特性进行详细解释及用法（看好了是部分，全部肯定是一时半会写不完的，想全方面去了解的话，建议读一下《ECMAScript 6 入门》）

---

### 下面开始开始正题（顺序无任何依据）：
#### 1.Default Parameters（默认参数）
ES5：


```
var link = function (height, color, url) {  
    var height = height || 50;  
    var color = color || 'red';  
    var url = url || 'http://azat.co';  
    ...  
}
```


ES6：直接写在参数里


```
var link = function(height = 50, 
    color = 'red',
    url = 'http://azat.co') {  
  ...  
}
```
++优点：节约代码量++
#### 2.Template Literals（模板对象）
ES5：


```
var name = 'Your name is ' + first + ' ' + last + '.';  
var url = 'http://localhost:8080/index/' + id;
```


ES6：，使用新的语法 $ {NAME}，并把它放在反引号里：


```
var name = 'Your name is ${first} ${last}.';
var url = 'http://localhost:8080/index/${id}';
```
++优点：这里的$ {NAME}直接当做字符串用，无需写加号，不容易乱++

#### 3.Multi-line Strings （多行字符串）
ES5:


```
var roadPoem = 'Then took the other, as just as fair,nt'  
    + 'And having perhaps the better claimnt'  
    + 'Because it was grassy and wanted wear,nt'  
    + 'Though as for that the passing therent'  
    + 'Had worn them really about the same,nt';  
var fourAgreements = 'You have the right to be you.n  
    You can only be you when you do your best.';
```


ES6: 反引号就可以啦！


```
var roadPoem = `Then took the other, as just as fair, 
    And having perhaps the better claim 
    Because it was grassy and wanted wear  
    Though as for that the passing theren 
    Had worn them really about the same,`;  
var fourAgreements = `You have the right to be you.n  
    You can only be you when you do your best.`;
```

++优点：直接一个反引号，将所有的字符串放进去即可，中介随意换行，清爽简洁（vue里面的template就这么用）++

####  4.Destructuring Assignment （解构赋值）

下边例子中，house 和 mouse是 key，同时 house 和 mouse 也是一个变量。

ES5：


```
var data = $('body').data(), // data has properties house and mouse  
    house = data.house,  
    mouse = data.mouse;
```

    
以及在node.js中用ES5是这样：


```
var jsonMiddleware = require('body-parser').jsonMiddleware ;  
var body = req.body, // body has username and password  
username = body.username,  
password = body.password;
```


ES6：


```
var {house,mouse} = $('body').data(); //we'll get house and mouse variables 
var {jsonMiddleware} = require('body-parser');
var {username,password} = req.body;
```


在数组中是这样的：


```
var [col1,col2] = $('.column'),
    [line1,line2,line3, ,line5] = file.split('n');
```
++优点：使用{}省去了写对象的属性的步骤，当然这个{}中的变量是与对象的属性名字保持一致的情况下。++

####  5.Enhanced Object Literals （增强的对象字面量）

使用对象文本可以做许多让人意想不到的事情！通过ES6，我们可以把ES5中的JSON变得更加接近于一个类。

下面是一个典型ES5对象文本，里面有一些方法和属性：


```
var serviceBase = {port: 3000, url: 'azat.co'},  
    getAccounts = function(){return [1,2,3]};  
var accountServiceES5 = {  
  port: serviceBase.port,  
  url: serviceBase.url,  
  getAccounts: getAccounts,  
  toString: function() {  
      return JSON.stringify(this.valueOf());  
  },  
  getUrl: function() {return "http://" + this.url + ':' + this.port},  
  valueOf_1_2_3: getAccounts()  
}
```


如果我们想让它更有意思，我们可以用Object.create从serviceBase继承原型的方法：


```
var accountServiceES5ObjectCreate = Object.create(serviceBase)  
// Object.create() 方法创建一个拥有指定原型和若干个指定属性的对象。
var accountServiceES5ObjectCreate = {  
  getAccounts: getAccounts,  
  toString: function() {  
    return JSON.stringify(this.valueOf());  
  },  
  getUrl: function() {return "http://" + this.url + ':' + this.port},  
  valueOf_1_2_3: getAccounts()  
}
```
  

ES6的对象文本中：既可以直接分配getAccounts: getAccounts,也可以只需用一个getAccounts


```
var serviceBase = {port: 3000, url: 'azat.co'},
getAccount = function(){return [1,2,3]};
var accountService = {
    __proto__: serviceBase, //通过proto设置属性
    getAccount, // 既可以直接分配getAccounts: getAccounts,也可以只需用一个getAccounts
    toString() { //这里将json形式改为函数形式 
        return JSON.stringify(super.valueOf()); 
        //调用super防范
    },  
    getUrl() {return "http://" + this.url + ':' + this.port},  
    [ 'valueOf_' + getAccounts().join('_') ]: getAccounts()  //使用动态key值(valueOf_1_2_3)此处将getAccounts()方法得到的数组[1,2,3]转化为字符串1_2_3
};
console.log(accountService);
```
优点：
- 相当于直接将结果写进去，而不再必须 key：value
- 将toString: function(){}这种json形式转变为 toString() {}这样的函数(类)的形式
- 既可以直接分配getAccounts:getAccounts这样的json形式，也可以只需用一个getAccounts表达相同的意思

#### 　6.Arrow Functions in（箭头函数）

这些丰富的箭头是令人惊讶的因为它们将使许多操作变成现实，比如， 
以前我们使用闭包，this总是预期之外地产生改变，而箭头函数的迷人之处在于，现在你的this可以按照你的预期使用了，身处箭头函数里面，this还是原来的this。

ES5:


```
var _this = this;  
$('.btn').click(function(event){  
  _this.sendData();  
})
```
 

ES6: 就不需要用 _this = this：


```
$('.btn').click((event) =>{  
  this.sendData();  
})
```


再比如：

ES5:


```
var logUpperCase = function() {  
  var _this = this;   //this = Object {string: "ES6 ROCKS"}
  console.log('this指的是',this); //Object {string: "ES6 ROCKS"}
  console.log('_this指的是',_this);//Object {string: "ES6 ROCKS"}
  this.string = this.string.toUpperCase();  
  console.log(_this.string); //ES6 ROCKS  
  console.log(this.string);  //ES6 ROCKS
  return function () {  
    return console.log(_this.string); //ES6 ROCKS
    return console.log(_this.string); //如果return _this.string，将返回 undefined，因为

  }  
} 
logUpperCase.call({ string: 'ES6 rocks' })();
```


ES6：我们并不需要用_this浪费时间,现在你的this可以按照你的预期使用了，身处箭头函数里面，this还是原来的this


```
var logUpperCase = function() {  
  this.string = this.string.toUpperCase();//this还是原来的this  
  return () => console.log(this.string);  
}  
logUpperCase.call({ string: 'ES6 rocks' })();
```


注意 只要你愿意，在ES6中=>可以混合和匹配老的函数一起使用。当在一行代码中用了箭头函数，它就变成了一个表达式。它将暗地里返回单个语句的结果。如果你超过了一行，将需要明确使用return。

ES5：


```
var ids = ['5632953c4e345e145fdf2df8','563295464e345e145fdf2df9'];  
var messages = ids.map(function (value) {  
  return "ID is " + value; // explicit return  
});
```


ES6：


```
var ids = ['5632953c4e345e145fdf2df8','563295464e345e145fdf2df9'];
var messages = ids.map((value)  => `ID is ${value}`); //implicit return
```
优点：
1. 并不需要用_this浪费时间，现在你的this可以按照你的预期使用了，身处箭头函数里面，this还是原来的this。 
1.  => 可以代替function关键字，当在一行用了箭头函数，可以省去{}，还可以省去return，它会暗地里返回的。

#### 　7.Promises
ES5：


```
setTimeout(function(){  
  console.log('Yay!');  
}, 1000);
```


ES6: 我们可以用promise重写


```
var wait1000 = new Promise((resolve,reject)=> {
   setTimeout(resolve,1000);
}).then(()=> {
    console.log('Yay!'); 
});
```


如果我们有更多的嵌套逻辑在setTimeout()回调函数中，好处会明显一点： 
ES5：


```
setTimeout(function(){  
  console.log('Yay!');  
  setTimeout(function(){  
    console.log('Wheeyee!');  
  }, 1000)  
}, 1000);
```


ES6: 我们可以用promise重写


```
var wait1000 = ()=> new Promise((resolve,reject)=>{ setTimeout(resolve,1000);});
wait1000()
    .then(function(){
        console.log('Yay!');  
        return wait1000()
    })
    .then(function(){
         console.log('Wheeyee!');  
    });
```

#### 　 8.Block-Scoped(块作用域和构造let和const）
let是一种新的变量声明方式，它允许你把变量作用域控制在块级里面。我们用大括号定义代码块，在ES5中，块级作用域起不了任何作用：


```
function calculateTotalAmount (vip) {  
  var amount = 0;  
  if (vip) {  
    var amount = 1;  
  }  
  { // more crazy blocks!  
    var amount = 100;  
    {  
      var amount = 1000;  
    }  
  }    
  return amount;  
}  
console.log(calculateTotalAmount(true));  // 1000
```


ES6: 用let限制块级作用域


```
function calculateTotalAmount(vip){
    var amouont  = 0; // probably should also be let, but you can mix var and let
    if (vip) {  
        let amount = 1; // first amount is still 0  
    }   
    { // more crazy blocks!  
    let amount = 100; // first amount is still 0  
    {  
      let amount = 1000; // first amount is still 0  
    }  
  }    
  return amount;  
} 
console.log(calculateTotalAmount(true));  //0
```
因为块作用域中有了let。
 
谈到const，就更加容易了；它就是一个不变量，也是块级作用域就像let一样。


#### 　9.Classes （类）

ES6没有用函数,而是使用原型实现类。我们创建一个类baseModel ，并且在这个类里定义了一个constructor 和一个 getName()方法：


```
class baseModel {  
    constructor(options, data) {// class constructor， 注意我们对options 和data使用了默认参数值。
        this.name = 'Base';  
        this.url = 'http://azat.co/api';  
        this.data = data;  
        this.options = options;  
   }  
    getName() { // class method  
        console.log(`Class name: ${this.name}`);  
    } 
    getUrl() { // class method  
         console.log(`Url: ${this.url}`);  
    }
}
```
 

AccountModel 从类baseModel 中继承而来:


```
class AccountModel extends baseModel {  
    constructor(options, data) { 
    super({private: true}, ['32', '5242']); 
    this.url +='/accounts/';  
    }
    get accountsData() {
        return this.data;  
    }  
} 
// 调用
let accounts = new AccountModel(5);  
accounts.getName();  // Class name:  Base
console.log('Data is %s', accounts.accountsData); 
// Data is 32,5242

//子类必须在constructor方法中调用super方法，否则新建实例时会报错。
//这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。
//如果不调用super方法，子类就得不到this对象。

```

#### 10. Modules （模块）

ES5导出：


```
module.exports = { port: 3000, getAccounts: function() { ... }}
```


ES6导出：


```
export var port = 3000;
export function getAccounts(url) { ...}
```


ES5导入：


```
var service = require('module.js');
console.log(service.port); // 3000
```


ES6导入：

我们需用import {name} from ‘my-module’语法


```
import {port, getAccounts} from 'module';
console.log(port); // 300
```


或者ES6整体导入：


```
import * as service from 'module';
console.log(service.port); // 3000
```

参考：
1. http://blog.csdn.net/bingtangcsnd/article/details/63684142
1. 《ES6 标准入门（第3版）》
