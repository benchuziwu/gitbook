http://stackoverflow.com/questions/22537311/angular-ui-router-login-authentication

angularjs权限与授权

权限指令：

1.principal：这个service服务存储用户的身份id，可以被检测查看用户是否已登录，它可以返回一个对象（代表关于用户身份id的必要的信息），principal也有方法做角色检测

```javascript
.factory('principal', ['$q','$http','$timeout', function($q,$http,$timeout){
     var _identity=undefined;
        _authenticated=false;
       return{
       	isIdentityResolved:function(){
       		return angular.isDefined(_identity);
       	},
       	isAuthenticated:function(){
       		return _authenticated;
       	},
       	isInRole:function(role){
            if (!_authenticated || !_identity.roles) 
            	return false;
       	},
       	isInAnyRole:function(roles){
            if (!_authenticated || !_identity.roles) return false;
            for (var i = 0; i < roles.length; i++) {
            	if (this.isInRole(roles[i])) return true;
            }
            return false;
       	},
       	authenticate:function(identity){
             _identity=identity;
             _authenticated=identity!=null;
       	},
       	identity:function(force){
              var deferrd =$q.defer();
              if (force===true) 
              	_identity=undefined;
       	}
       	if (angular.isDefined(_identity)) {
       		deferred.resolve(_identity);
       		return deferred.promise;
       	}
       	var self=this;
       	$timeout(function(){
       		self.authenticate(null);
       		deferred.resolve(_identity);
       	},1000);
       	return deferred.promise;
       }
       };
}
])
```

2.需要一个service检测用户想要干嘛的state状态，确定他们是登录的，然后做一个角色检测，如果他们未认证，发送给他们一个登录页面，如果他们认证了，但是角色检测失败，发送一个拒绝访问页面给用户，我称这个服务service为authorization

```javascript

.factory('authorization', ['$rootScope','$state','principal', 
	function($rootScope,$state,principal){

       return{
       	authorize:function(){
       		return principal.identity().then(function(){
       			 var isAuthenticated=principal.isAuthenticated();
       			 if ($rootScope.toState.data.roles && $rootScope.toState.data.roles.length>0 
       			 	&& !principal.isInAnyRole($rootScope.toState.data.roles)) {
   if (isAuthenticated) {   
     // user is signed in but not
       // authorized for desired state
     $state.go('accessdenied')
       			 	} 
    else{
       			 		$rootScope.returnToState=$rootScope.toState;
       			 		$rootScope.returnToStateParams=$rootScope.toStateParams;
       			 		$state.go('signin');
       			 	}
       			 }
       		})
       	}
       }

}
])
```

接下来你所有要做的是监听我们的ui-router的$stateChangeStart,这个给你一个机会去测试目前的状态，他们想要前往的路由状态，并且注入你的anthorization 检测。如果失败了，你可以取消路由转变，或者改变去一个不同的路由

```javascript
.run( [''$rootScope','$state', '$stateParams','authorization','principal' ,', function($rootScope, $state, $stateParams, 
             authorization, principal){

	$rootScope.$on('$stateChangeStart', 
	    function(event, toState, toStateParams)
	{
	  // track the state the user wants to go to; 
	  // authorization service needs this
	  $rootScope.toState = toState;
	  $rootScope.toStateParams = toStateParams;
	  // if the principal is resolved, do an 
	  // authorization check immediately. otherwise,
	  // it'll be done when the state it resolved.
	  if (principal.isIdentityResolved()) 
	      authorization.authorize();
	});
	
}])
```

3.关于追踪用户的身份最困难的部分是检测你是否早就认证过，如果你先前（session）访问了页面，

保存了一个authtoken在一个cookie中，或许你很难再刷新一个页面，或者离开通过一个URL链接打开，因为ui-router工作，在你auth 检测之前，你需要再次认证你的身份（identity resolve），你可以使用resolve选项做这个

```
$stateProvider.state('site', {
  'abstract': true,
  resolve: {
    authorize: ['authorization',
      function(authorization) {
        return authorization.authorize();
      }
    ]
  },
  template: '<div ui-view />'
})
```

```
每一种角色对应一组相应的权限，一旦用户被分配了适当的角色后，该用户就拥有此角色的所有操作权限，这样的好处是不比在每次创建用户时都进行分配权限的操作，只需分配用户相应的角色即可，
1.ui处理(根据用户拥有的权限，判断页面上的一些内容是否显示)
2.路由处理(当用户访问一个它没有权限访问的URL时，跳转到一个错误提示的页面)
3.http请求处理(当我们发送一个数据请求，如果返回的status是401或者403.则通常重定向到一个错误提示的页面)
首先需要在angular启动之前就获取到当前用户的所有permissions，然后比较优雅的方式是通过一个service存放这个映射关系，
最后还需要一个http拦截器监控当一个请求返回的status是401或者403时，跳转页面到一个错误提示页面
  Angular项目通过ng-app启动,但是一些情况下我们是希望 Angular项目的启动在我们的控制之中.比如现在这种情况下,我就希望能获取到当前登录用户的所有permission映射关系后,再启动 Angular的App.幸运的是Angular本身提供了这种方式,也就是angular.bootstrap().
```

```
$q是Angular的一种内置服务，它可以使你异步地执行函数，并且当函数执行完成时它允许你使用函数的返回值（或异常）,defer的字面意思是延迟， $q.defer()  可以创建一个deferred实例（延迟对象实例）。deferred 实例旨在暴露派生的Promise 实例,
```

```
var deferred = $q.defer();  //通过$q服务注册一个延迟对象 deferred
var promise = deferred.promise;  //通过deferred延迟对象，可以得到一个承诺promise，而promise会返回当前任务的完成结果
 deferred.resolve(value)  成功解决(resolve)了其派生的promise
```

Role-Based Access Control