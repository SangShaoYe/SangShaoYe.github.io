---
title: webpack
date: 2018-07-16 11:11:09
tags: webpack
---

angular webpack

author：桑景瑞
<!-- more -->


```
var path = require('path');

var HtmlwebpackPlugin = require('html-webpack-plugin');

var webpack=require('webpack');

//定义了一些路径

var APP_PATH = path.resolve(__dirname, 'src/js/app.js');

var BUILD_PATH = path.resolve(__dirname, 'build');

module.exports = {

//入口，分为app 入口和提取插件库的入口

entry:{

  app:APP_PATH,

  vendors:["angular","angular-animate","angular-route","angular-resource","jquery"],

},

resolve:{

//定义别名，让require引用引入min文件打包的引入压缩后的文件，减少打包后文件的体积

alias:{

   angular:path.resolve(__dirname,"node_modules/angular/angular.min.js"),

   "angular-animate":path.resolve(__dirname,"node_modules/angular-animate/angular-animate.min.js"),

   "angular-route":path.resolve(__dirname,"node_modules/angular-route/angular-route.min.js"),

   "angular-resource":path.resolve(__dirname,"node_modules/angular-resource/angular-resource.min.js"),

  jquery:path.resolve(__dirname,"node_modules/jquery/dist/jquery.min.js")

}

},

//输出文件路径和名字

output: {

  path: BUILD_PATH,

  filename: 'bundle.js'

},



plugins: [

//添加我们的插件 提取js插件库

  new webpack.optimize.CommonsChunkPlugin('vendors', 'vendors.js'),

//声明jQuery全局插件

  new webpack.ProvidePlugin({

  $: "jquery",

  jQuery: "jquery"

})

],

//dev 服务器

devServer: {

  historyApiFallback: true,

  hot: true,

  inline: true,

  progress: true,

  contentBase:"./build"//dev server的根路径

},

//加载器

"module": {

  "loaders": [

    {test: /\.css$/,loader:'style!css'},

    {

      test: /\.(png|jpg|woff|svg|ttf|eot)$/,

      loader: 'url?limit=40000'

  },

  {

    test:/\.js?$/,

    loader: 'babel',

    include: APP_PATH,

    query: {

      presets: ['es2015']

    }

  },

  {

    test: /\.html$/, // Only .html files

    loader: 'raw' // Run html loader

  }

  ]

  },

devtool:'eval-source-map'

};
```


然后phoneCat的js文件 入口文件添加引用require("angular")等引用

注意：请不要写成 var angular = require("angular")；这样运行的的时候会报错，


angular.module 未定义，因为这里的angular是你定义的angular变量。

index.html


```
<!doctype html>
<html lang="en" ng-app="phonecatApp">
<head>
  <meta charset="utf-8">
  <title>Google Phone Gallery</title>
  <script src="vendors.js"></script>
  <script src="bundle.js"></script>
</head>
<body>

  <div class="view-container">
    <div ng-view class="view-frame"></div>
  </div>

</body>
</html>
```

app.js


```
'use strict';

//这里引入所需库，其他页面就不需要引入了
require('angular') ;
require('angular-route');
require('angular-animate'); 
require('angular-resource'); 

//这里引入项目所需要的js文件和css文件所有页面没有修改成模块化，
require('./controllers.js'); 
require('./animations.js');
require('./filters.js');
require('./services.js');
require('../css/app.css');
require('../css/animations.css');
require('../../node_modules/bootstrap/dist/css/bootstrap.css');


var phonecatApp = angular.module('phonecatApp', [
  'ngRoute',
  'phonecatAnimations',
  'phonecatControllers',
  'phonecatFilters',
  'phonecatServices'
]);

phonecatApp.config(['$routeProvider',
  function($routeProvider) {
    $routeProvider.
      when('/phones', {
        template: require('../partials/phone-list.html'),
        controller: 'PhoneListCtrl'
      }).
      when('/phones/:phoneId', {
        template: require('../partials/phone-detail.html'),
        controller: 'PhoneDetailCtrl'
      }).
      otherwise({
        redirectTo: '/phones'
      });
  }]);
module.exports = phonecatApp;
```

其他的文件没有改动，我把资源文件夹和img文件夹放到了build文件夹中这样启动项目就可以了

自己百度整理的webpack配置

context: 上下文，
    entry: 入口文件，单入口写字符串，多入口写数组，也可以写成

    output:打包出口文件路径
    output.filename： 文件名
    output.path: 出口文件路径
    output.publicPath:可以替换代码中的路径，比如所有图片都发布在文件服务器，你可以吧文件服务器的地址放到这里，'/img/a.png'会替换成'http://xx.xx/img/a.png'
    output.chunkFilename：非入口文件的命名规则
    output.sourceMapFilename：sourcemap文件名称
    
    module.loaders ：各种各样的加载器
        每一个loader 都有这几项配置
        test: 一般写正则匹配要使用的loader
        exclude: A condition that must not be met 排除这些文件不用进行loader
        include: An array of paths or files where the imported files will be transformed by the loader
        loader: A string of “!” separated loaders
        loaders: An array of loaders as string 一个loader的数组
    module.preLoaders 最先进行的loader 一般最先开始进行，一般用于eslint格式检查
    module.postLoaders 最后进行的loader
    module.noParse  如果你 确定一个模块中没有其它新的依赖 就可以配置这项，webpack 将不再扫描这个文件中的依赖

    resolve.alias 可以重定向require文件的路径 比如 moment: "moment/min/moment-with-locales.min.js" require('moment') 回引用moment.min 文件
    resolve.root （必须是绝对路径） 表示添加默认搜索路径
    resolve.modulesDirectories 默认值["web_modules", "node_modules"] 
    resolve.extensions 参数指明那些文件 Webpack 是要搜索的  ['', '.js', '.json', '.coffee']   现在可以写 require('file') 代替 require('file.coffee')
    

    externals 外部依赖项，这里可以使用CDN中的资源比如：moment: true 需要在html中加入CDN的引用<script src="//apps.bdimg.com/libs/moment/2.8.3/moment-with-locales.min.js"></script>
    
    target 编译时使用什么环境 默认web环境

    cache 编译缓存 默认开启，增量编译速度变快，也可以关闭
    debug 开发模式
    
    devtool 各种模式各有有优缺点