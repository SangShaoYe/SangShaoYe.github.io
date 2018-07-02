---
title: AngularJS中的http拦截
date: 2018-05-19 12:22:07
tags: angular
---

$http服务允许我们与服务端交互，有时候我们希望在发出请求之前以及收到响应之后做些事情。即http拦截。

author：桑景瑞
<!-- more -->

$http服务允许我们与服务端交互，有时候我们希望在发出请求之前以及收到响应之后做些事情。即http拦截。

$httpProvider包含了一个interceptors的数组。

我们这样创建一个interceptor。
 
```
app.factory('myInterceptor', ['$log', function($log){
    $log.debug('');
    
    var myInterceptor = {};
    
    return myInterceptor;
}])
```
 
接着注册interceptor.
```
app.config(['$httpProvider', function($httpProvider){
    $httpProvider.interceptors.push('myInterceptor');
}])
```
以下是$http拦截的一些例子。

■ 拦截器中的异步操作
 
```
app.factory('myInterceotpr','someAsyncServcie', function($q, someAsyncServcie){
    var requestInterceptor = {
        request: function(config){
            var deferred = %q.defer();
            someAsyncService.doAsyncOperation().then(function(){
                ...
                deferred.resolve(config);
            }, function(){
                ...
                deferred.resolve(config);
            })
            return deferred.promise;
        }
    };
    
    return requestInterceptor;
})
```
以上，是一个请求拦截，做了一个异步操作，根据异步操作的结果来更新config。

当然也有响应拦截。
 
```
app.factory('myInterceptor',['$q', 'someAsyncService', function($q, someAsyncSercice){
    var responseInterceptor = {
        response: function(response){
            var deferred = $q.defer();
            someAsyncService.doAsyncOperation().then(function(response){
                ...
                deferred.resolve(response);
            }, function(response){
                ...
                deferred.resolve(response);
            })
            return deferred.promise;
        }
    };
    return responseInterceptor;
}])
```
 
■ Session拦截，请求拦截

服务端有2种类型的验证，一个是基于cookie的，一种是基于token的。对于基于token验证，当用户登录，获取一个来自服务端的token，这个token在每一次请求时发送给服务端。

创建一个有关session的injector:
```
app.factory('sessionInjector',['SessionService', function(SessionService){
    var sessionInjector = {
        request: function(config){
            if(!SessionService.isAnonymous){
                config.headers['x-session-token'] = SessionService.token;
            }
            return config;
        }
    };
    
    return sessionInjector;
}])
```
可见，把从服务端返回的token放在了config.headers中。

注册injector:
```
app.config(['$httpProvider', function($httpProvider){
    $httpProvider.interceptors.push('sessionInjector');
}])
```
发出一个请求：
```
$http.get('');
```
拦截前大致是：
```
{
    "transformRequest":[null],
    "transformResponse":[null],
    "method":"GET",
    "url":"",
    "headers":{
        "Accept": "application/json, text/plain,*/*"
    }
}
```
拦截后，在headers中多两个一个x-session-token字段:
{
```
    "transformRequest":[null],
    "transformResponse":[null],
    "method":"GET",
    "url":"",
    "headers":{
        "Accept": "application/json, text/plain,*/*",
        "x-session-token":......
    }
}
```

■ 时间戳，请求和响应拦截
 
```
app.factory('timestampMarker',[function(){
    var timestampMarker = {
        request:function(config){
            config.requestTimestamp = new Date().getTime();
            return config;
        },
        response: function(response){
            response.config.responseTimestamp = new Date().getTime();
            return config;
        }
    };
    
    return timestampMarker;
}])
```
以上，在请求和响应时拦截，在config.requestTimestamp和config.responseTimestamp赋上当前的时间。

注册拦截器：
```
app.config(['$httpProvider', function($httpProvider){
    $httpProvider.interceptors.push('timestampMarker');
}])
```
然后在运用的时候可以算出请求响应所耗去的时间。
```
$http.get('').then(function(response){
    var time = response.config.responseTime - response.config.requestTimestamp;
    console.log('请求耗去的时间为 ' + time);
})
```

■ 请求错误恢复，请求拦截

模拟一个请求拦截的错误情形：
 
```
app.factory('requestRejector',['$q', function($q){
    var requestRejector = {
        request: function(config){
            return $q.reject('requestRejector');
        }
    };
    return requestRejector;
}])
```
 
拦截请求错误：
```
app.factory('requestRecoverer',['$q', function($q){
    var requestRecoverer = {
        requestError: function(rejectReason){
            if(rejectReason === 'requestRejector'){
                //恢复请求
                return {
                    transformRequest:[],
                    transformResponse:[],
                    method:'GET',
                    url:'',
                    headers:{
                        Accept:'application/json, text/plain, */*'
                    }
                };
            } else {
                return $q.reject(rejectReason);
            }
        }
    };
    
    return requestRecoverer;
}])
```
 
注册拦截器：
```
app.config(['$httpProvider', function($httpProvider){
    $httpProvider.interceptors.push('requestRejector');
    $httpProvider.interceptors.push('requestRecoverer');
}])
```
■ Session错误恢复，响应拦截
 
```
app.factory('sessionRecoverer',['$q','$injector',function($q, $injector){
    var sessionRecoverer = {
        responseError: function(response){
            //如果Session过期
            if(response.status == 419){
                var SessionService = $injector.get('SessionService');
                var $http = $injector.get('$http');
                var deferred = $q.defer();
                
                //创建一个新的session
                SessionService.login().then(deferred.resolve, deferred.reject);
                
                return deferred.promise.then(function(){
                    reutrn $http(response.config);
                })
            }
            return $q.reject(response);
        }
    };
    
    return sessionRecoverer;
}])
```