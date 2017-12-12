# AOP

AOP(Aspect-Oriented Programming)，面向切面编程。

通过compile时植入代码，runtime时动态代理，以及框架提供管道式执行等策略实现程序**通用功能与业务模块的分离**，统一处理，维护的一种**解耦设计**。

AOP实用场景有：权限控制，日志模块，事务处理，性能统计，异常处理等**独立**，**通用**的**非业务**模块。+

在ng中做页面访问权有很多方法，运用较多的是拦截器。拦截器使得在前端往后端发送http请求之前或之后做一些操作，比如全局监测用户是否登录，没登录就要跳转登录页面，登录就可以访问页面，拦截器的使用往往配合后台数据，也就是获取到最新的标识，

1.http拦截器(管道式策略)

```
/
/定义拦截器
angular.module('myApp')
.factory('myInterceptor',
function($q) {

var interceptor={
'request': request,
'response':response,
'requestError':requestError,
'responseError':responseError
}

return interceptor;

function request(config){
// Successful request method
return config; // or $q.when(config);
}
function response(response){
return response; // or $q.when(response);

}
function requestError(rejection){
return $q.reject(rejection);
}
function responseError(rejection){
// an error happened on the request if we can recover from the error,
// we can return a new response or promise
return rejection; // or new promise
// Otherwise, we can reject the next by returning a rejection
// return $q.reject(rejection);
}

// 使用拦截器
angular.module('myApp')
.config(function($httpProvider) {
$httpProvider.interceptors.push('myInterceptor');
});
```

拦截器提供了对发出请求到返回响应的全生命周期处理，一般可以用来做下面几个事情：

```
1
.统一在发出的请求中添加数据，如添加身份验证信息

2.统一处理错误，包括请求发出时出的错（如浏览器端的网络不通），还有响应时返回的错误
3.统一处理响应，比如缓存一些数据等
4.显示请求进度条
其实，只要服务器返回的状态码不是 200 ，都会调用 responseError ，可以在这里，统一处理并显示错误。
```

ejt项目中用到aop编程的地方主要有统一给所有请求头添加Authorization和配置Accept-Language，统一处理错误状态码，给请求过程中添加loading的效果，log打印。主要代码在interceptor下。