Promise是一种异步处理模式，

#### $q常用的几个方法

一个帮助处理异步执行函数的服务。当他们做完处理时，使用它们的返回值（或异常）

```
defer() 创建一个deferred对象，这个对象可以执行几个常用的方法，比如resolve,reject,notify等
all() 传入Promise的数组，批量执行，返回一个promise对象
when() 传入一个不确定的参数，如果符合Promise标准，就返回一个promise对象。
```

在Promise中，定义了三种状态：等待状态，完成状态，拒绝状态。

**defer()方法**

在$q中，可以使用resolve方法，变成完成状态；使用reject方法，变成拒绝状态。其中defer()用于创建一个deferred对象，defer.promise用于返回一个promise对象，来定义then方法。then中有三个参数，分别是成功回调、失败回调、状态变更回调。

```javascript
<html ng-app="myApp">
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
  <script src="http://apps.bdimg.com/libs/angular.js/1.2.16/angular.min.js"></script>
</head>
<body>
  <div ng-controller="myctrl">
    {{test}}
  </div>
  <script type="text/javascript">
     var myAppModule = angular.module("myApp",[]);
angular.module("myApp",[]).controller("myctrl",['$scope','$q',function($scope, $q ){
      $scope.test = 1;//这个只是用来测试angularjs是否正常的，没其他的作用
 
      var defer1 = $q.defer();
      var promise1 = defer1.promise;
      promise1
      .then(function(value){
        console.log("in promise1 ---- success");
        console.log(value);
      },function(value){
        console.log("in promise1 ---- error");
        console.log(value);
      },function(value){
        console.log("in promise1 ---- notify");
        console.log(value);
      })
      .catch(function(e){
        console.log("in promise1 ---- catch");
        console.log(e);
      })
      .finally(function(value){
        console.log('in promise1 ---- finally');
        console.log(value);
      });
 
      defer1.resolve("hello");
      // defer1.reject("sorry,reject");
     }]);
  </script>
</body>
</html>
```



**all()方法**

这个all()方法，可以把多个promise的数组合并成一个。当所有的promise执行成功后，会执行后面的回调。回调中的参数，是每个promise执行的结果。

```javascript
var funcA = function(){
     console.log("funcA");
     return "hello,funA";
   }
   var funcB = function(){
     console.log("funcB");
     return "hello,funB";
   }
   $q.all([funcA(),funcB()])
   .then(function(result){
     console.log(result);
   });
```

执行的结果：

```
funcA
funcB
Array [ "hello,funA", "hello,funB" ]
```

**when()方法**

when方法中可以传入一个参数，这个参数可能是一个值，可能是一个符合promise标准的外部对象。 当传入的参数不确定时，可以使用这个方法。

```javascript
var funcA = function(){
     console.log("funcA");
     return "hello,funA";
   }
   $q.when(funcA())
   .then(function(result){
     console.log(result);
   });
```

当传入的参数不确定时，可以使用这个方法。

hello,funA

