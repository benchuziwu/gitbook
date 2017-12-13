# angular-ui-router

AngularUI库提供的最有用的库之一便是ui-router。它是一个路由框架，允许你通过状态机组织接口，而不是简单的URL路由。

### 1.安装ui-router

```
 bower install angular-ui-router --save
```

### 2.引入

 ```
<script src="app/bower_components/angular-ui-router/release/angular-ui-router.js"></script>
 ```

### 3. 注入依赖

```
angular.module('myApp', ['ui.router']);
```

### 4.工作原理

ui-router基于状态工作,在处理ngRoute服务时我们不再使用ng-view，而改为使用ui-view指令。在ui-router内处理路由和状态时，我们主要关心的是应用程序处在哪个状态以及Web应用当前处在哪个路由位置。定义在任意给定状态内的模板都处在<div ui-view></div>元素内。此外，每个模板都可以包含自己的ui-view。 这事实上就允许你在路由中嵌套视图。

```
 <div ng-controller="DemoController">
         <div ui-view></div>
 </div>
```

### 5.配置ui-router

```
使用.config方法定义路由,状态设置在$stateProvider上。
.config(function($stateProvider,$urlRouterProvider) { $stateProvider
         .state('start', {
           url: '/start',
           templateUrl: 'partials/start.html'
}) })
这一步给状态配置对象分配了一个名为start的状态
```

### 6.ui-router参数解释

```
1. template、templateUrl、templateProvider
 template:一个HTML内容字符串或者返回HTML的函数。
    template: '<h1>Hello {{ name }}</h1>'
 templateUrl:一个模板URL路径字符串或者是返回URL路径字符串的函数()
templateProvider:一个返回HTML内容字符串的函数
```

```
2. controller:给该路由定义一个控制器
```

```
3.resolve
使用resolve功能解析要注入到控制器中的依赖列表
在ngRoute中，resolve选 项允许你在路由被真实渲染之前解析promise。在angular-route内，对于可以如何使用这个选项 更自由。
这个resolve选项就是一个对象，其中键就是要注入到控制器中的依赖名称，而其值就是待 解析的factories。
如果传入一个字符串，angular-route会尝试匹配一个现有的已注册的服务。如果传入一个函数，则注入这个函数，而函数的返回值就是依赖。如果这个函数返回一个promise，它会在控制器被实例化之前解析，同时其值(就像ngRoute)会注入到控制器中。   
```

```
$stateProvider.state('home', {
         resolve: {
// 当结果不是promise时立即返回 
person: function() {
                 return {
                     name: "Ari",
                     email: "ari@fullstack.io"
                 }
},
// 这个函数返回一个promise，它会在控制器实例化之前解析 
currentDetails: function($http) {
                 return $http({
                     method: 'JSONP',
                     url: '/current_details'
}); },
// 还可以在另一个解析中使用上面返回的promise 
facebookId: function($http, currentDetails) {
                 $http({
                     method: 'GET',
                     url: 'http://facebook.com/api/current_user',
                     params: {
                         email: currentDetails.data.emails[0]
                     }
}); }
} });
```

```
4. url
url选项可以给应用程序的状态分配一个唯一的URL。这个url选项提供了与深度链接同样的功能，它通过状态导航应用
 基本路由可以像这样指定:
     $stateProvider
       .state('inbox', {
         url: '/inbox',
         template: '<h1>Welcome to your inbox</h1>'
     });
     当用户导航到/inbox时，应用会转换到inbox状态，然后使用模板内容(<h1>Welcome to your inbox</h1>)填充主要的ui-view指令。
     
  URL可以接受一系列不同的选项，它还可以在url中设置基本的参数，就像在ngRoute中一样:
     $stateProvider
       .state('inbox', {
         url: '/inbox/:inboxId',
         template: '<h1>Welcome to your inbox</h1>',
         controller: function($scope, $stateParams) {
             $scope.inboxId = $stateParams.inboxId;
         }
});

应用会捕获作为URL第二个组成部分的:inboxId。例如，如果用户转换到/inbox/1，
$stateParams.inboxId就会变成1(因为$stateParams为{inboxId: 1})。 如果你喜欢，还可以使用不同的语法:
url: '/inbox/{inboxId}'
```

```
5.abstract抽象模版  
抽象模板永远不能直接激活，但是可以设置被激活的子节点。
可以使用抽象模板提供一个模板包装器来包裹多个命名视图，或者传递$scope对象给子节 点。你还可以使用它们来传递解析后的依赖或者自定义数据，或者在同一url下嵌套多个路由(比 如，所有的路由都在/adminURL之下)。
设置抽象模板与设置常规状态一样，区别只在于设置abstract属性:

     $stateProvider
       .state('admin', {
           abstract: true,
           url: '/admin',
           template: ;<div ui-view></div>'
       })
       .state('admin.index', {
           url: '/index',
           template: '<h3>Admin index</h3>'
       })
或者
     $stateProvider
       .state('admin', {
           abstract: true,
           url: '/admin',
           template: ;<div ui-view></div>'
       })
       .state('index', {
           parent:admin,
           url: '/index',
           template: '<h3>Admin index</h3>'
       })
```

```
6.onEnter、onExit
Angular会在用户(分别)进入或者离开视图时调用这些回调函数。对于这两个选项，你可
以设置希望调用的函数。这些函数可以访问被解析的数据。
这些回调函数让你可以在新视图上或者进入另一个状态时触发某个行为。使用它们可以很好 地实现一个“你确定吗?”形式的模态视图，或者在用户进入这个状态之前要求用户登录。(ejt项目中多个类似的弹出模态层的方式就是用onEnter实现的）
        .state('alipay', {
          parent: 'payProcess',
          url: '/alipayProcess/:alipay',
          onEnter: ['$stateParams', '$state', '$uibModal', function ($stateParams, $state, $uibModal) {
            $uibModal.open({
              templateUrl: 'app/main/pay/alipay.html',
              controller: 'orderPayController',
              controllerAs: 'vm',
              backdrop: 'static',
              size: 'md'
            })
          }]
        })
```

```
7. data 你可以附加任意数据给你的状态配置对象configObject。这个选项跟resolve属性很像，但
是它的数据不会被注入到控制器中，promise也不会被解析。 当需要从父状态给子状态传递数据时，这个选项特别有用
```



### 7. 嵌套路由

```
$stateProvider
  .state('inbox', {
    url: '/inbox/:inboxId',
    template: '<div><h1>Welcome to your inbox</h1>\
        <a ui-sref="inbox.priority">Show priority</a>\
        <div ui-view></div>\
        </div>'
    controller: function($scope, $stateParams) {
        $scope.inboxId = $stateParams.inboxId;
} })
  .state('inbox.priority', {
      url: '/priority',
      template: '<h2>Your priority inbox</h2>'
});
inbox就是priority的父路由
```

### 8.路由传参($stateParams)

```
用$stateParams从URL的参数中辨别出不同的参数选项。这个 服务展示了如何根据URL的不同组成部分处理数据。
例如，如果在inbox状态中有个看起来像这样的URL: url: 'inbox/:inboxId/messages/{sorted}}?from&to'
然后用户到达这个路由:
/inbox/123/messages/ascending?from=10&to=20 那么$stateParams对象的结果就是:
{inboxId: '123', sorted: 'ascending', from: 10, to: 20}
```

### 9.$urlRouterProvider

```
创建的这些状态负责在不同的URL中激活自身，因此不一定需要$urlRouterProvider来管 理激活和加载状态。当你想要管理发生在状态作用域之外的行为时，它就可以派上用场了，比如 重定向或者身份验证。
可以在模块配置函数中使用$urlRouterProvider。

otherwise() 和ngRoute中的otherwise()方法一样，这个otherwise()方法在没有其他路 由匹配时发起重定向。这个方法是创建默认URL的一种很好的方式。
otherwise()方法接受一个参数:一个字符串或者函数。 如果传入一个字符串，任何无效或者不匹配的路由都会重定向到字符串指定的URL。 如果传入一个函数，它会在没有其他路由匹配时被调用，同时负责处理返回结果。
  $urlRouterProvider.otherwise('/login');
```

![](http://i4.buimg.com/567571/2d26ea80f227b72d.png)






​			
​		
​	
