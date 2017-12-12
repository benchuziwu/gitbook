# 权限文档

​                                         -------------------------Author by chenyanwen

> 当前的项目中在做权限认证的只有两处，且调用的是同一个方法，我们先从这里两处入手

### 项目的初始化(第一处)
整个项目在初始化的时候
```
module.run中
调用了
stateHandler.initialize();

位置：(src/main/webapp/app/blocks/handlers/state.handler.js)
```
在这个初始化函数中，开始对$stateChangeStart事件进行监听，这个监听事件在整个项目
初始化时候定义好，并且立即开始发挥作用
```
这个事件监听函数主要做了两件事情：

1.在$rootScope上根据回调函数的参数，挂载
toState， toStateParams， fromState

2.如果Principal服务中保存的变量_identity已经定义(与是否登录同步)
就进入Auth.authorize()方法。
```
```
*** Auth.authorize()方法是权限认证的唯一方法***

Principal服务位置：src/main/webapp/app/services/auth/principal.service.js
Auth.authorize()位置：src/main/webapp/app/services/auth/auth.service.js
```
由以上信息可知，这个权限鉴别会在每次更换路由开始的时候就调用，但是需要_identity存在的情况下才
会发挥作用。
### home.state路由的预处理(第二处)
这个路由是除了登录页之外的左右其他路由的父路由。
```
 authorize: ['Auth',
             function (Auth) {
               return Auth.authorize();
           }
      ]
```
我们可以发现，在这个路由的预处理函数中调用了authorize方法。
也就是我们之前提过的权限鉴别的唯一函数。

> 项目的权限鉴别在以上的两处配合完成保证完整性

1. 进入主页之后的权限鉴定，全部由$stateChangeStart事件监听来做。
2. 如果你想通过复制链接或者硬刷新等非登录途径进入项目，则会被home.state的预处理拦截。

### Auth.authorize()
整个项目唯一的权限鉴别函数都做了什么?
```
1. Auth.authorize()方法首先会调用Principal.identity()

Principal.identity()内部会对后端发起两个请求

//获取用户权限信息
Account.get().$promise.then(getAccountThen);
//获取该已注册用户及认证信息
UserInfo.get().$promise.then(getUserinfoThen);
2.Auth.authorize()拿到identity返回来的用户信息等等
根据业务逻辑判断哪些路由可以访问。
```
### 登录
我们来走一遍登录的过程。
```
调用控制器中的login   (src/main/webapp/app/components/login/login.controller.js)
|
|
|
调用Auth.login   (src/main/webapp/app/services/auth/auth.service.js)
|
|
|
调用AuthServerProvider.login   (src/main/webapp/app/services/auth/auth.jwt.service.js)
在这里向后端(api/authenticate)发送登录的相关信息(user,password,rememberme)
并且从返回的data中获取token
var jwt = data.koken;

然后根据remember参数决定token是保存在（localStorage和$rootScope）或者（sessionStrorage）
|
|
|
回到Auth.login
在这里调用Principal.identity(true),又重新获取了用户信息。
|
|
|
回到控制器中的login
$state.go(overview)
```
```
与此同时拦截器auth.interceptor.js   (src/main/webapp/app/blocks/interceptor/auth.interceptor.js)
会在每次的请求头中添加token
```
所以随着用户的一次登录，我们获得了该用户的所有信息，并且随着用户的每一次请求都会带有
他相关信息的token。
### 登出
登出就相对简单得多。
1. 删除本地缓存中的token。
2. 清空和取反principal中的_identity, _authenticated。
3. $state.go(login)。
### 目前需求的结局方案
需求1：登陆的时间过长需要重新登录

方案：在token上加上时间限制，由后端判断并返回相应的状态码。

需求2：如何根据动态新增的权限决定那些路由可以访问。

方案：后端返回的用户信息上新增路由列表，在authorize函数中根据这个
列表鉴权。


