# require.js的小应用

##### [AngularJS](https://angularjs.org/) 目前的版本没有遵循 Javascript 约定的 [AMD](https://github.com/amdjs/amdjs-api/wiki/AMD) 模块化规范， 因此使用 [RequireJS](http://requirejs.org/) 加载 AngularJS 时需要一些额外的配置。(shim)

###配置bower环境


``` mkdir bowerprojecttest
mkdir bowerprojecttest
cd bowerprojecttest
npm install -g bower
touch  .bowerrc//touch创建文件  更改使用bower下载安装js包时的路径,{"directory":"js/lib"}譬如使用bower安装angular后去文件夹该目录下寻找
cat .bowerrc 查看
bower init //Create a bower.json file for your package with bower init.
bower install PACKAGE --save//save new dependencies to your bower.json with bower install PACKAGE --save bower管理js包依赖
```

##### 关于bower install PACKAGE后面是否添加—save的区别

```
(以安装angular为例)
##添加－－save来安装包时全局下bower.json就会管理js包的依赖
    bower.json中dependencies是这样滴
      "dependencies": {
        "requirejs": "^2.3.2",
        "angular": "^1.5.8"
      }
##若不添加－－save则只下载包，而bower.json中就不会出现此包的管理依赖
    bower.json中dependencies是这样滴
        "dependencies": {
        "requirejs": "^2.3.2"
      }
  
```

### 介绍require.js

```
RequireJS是一个非常小巧的JavaScript模块载入框架
其主要目的还是为了代码的模块化,可以用它来加速、优化代码，
防止js加载阻塞页面渲染,使用程序调用的方式加载js,而不是传统的在首页引入很多js文件。
在使用脚本时以module ID替代URL地址。
```

### 安装require.js

```
bower install requirejs --save
```



## 以angular项目为例快速了解requirejs的用法

#### 使用bower安装angularjs管理依赖

```
bower install angular --save
```

#### 在index.html页面引入require.js和config.js的路径(自定义)

```
//加载requirejs脚本的script标签加入了data-main属性，这个属性指定的js将在加载完reuqire.js后处理，我们把require.config的配置加入到data-main后，就可以使每一个页面都使用这个配置，然后页面中就可以直接使用require来加载所有的短模块名
<script src="js/lib/requirejs/require.js" data-main="config.js"></script>
```

###配置config.js文件:require加载文件(写在根目录下)

##### 写法1

```
//require会定义三个变量：define,require,requirejs，其中require === requirejs，一般使用require更简短
require.config({
    baseUrl: 'js/lib',
	paths:{
	angular:'/angular/angular',//本地安装的angular路径
	app:'myapp'//譬如此demo是angular，myapp.js则写关于页面的控制器代码
	shim: {
  	angular : { exports : 'angular'}
	}
	})
require(['app'],function(app){／／注意require中的依赖是一个数组
})//通过require加载的模块一般都需要符合AMD规范即使用define来申明模块
```

###### 写法2[^shim]

```
require.config({
   baseUrl: 'js/lib',
	paths:{
		angular:'/angular/angular',
		app:'myapp'
	},
	  shim: {
  	   angular : { exports : 'angular'}
  },
  deps:['app']//启动程序 myapp.js
});
```

###### 关于AMD[^AMD]

### 关于此部分angular的延伸

```
require.config({
    //配置总路径
    baseUrl : "js/scripts",
 
    paths : {
        // 其他模块会依赖他
        'ui.route':'../lib/angular-plugins/angular-ui-router/angular-ui-router.min',
        'angular' : '../lib/angular/angular',
        'angular-route' : '../lib/angular-route/angular-route',
        'angularAMD' : '../lib/angular-plugins/angularAMD',
        'css' : '../lib/requirejs/css.min',
        'jquery' : '../lib/jquery.min',
        'ngload':'../lib/angular-plugins/ngload',
        'ui-bootstrap':'../lib/angular-plugins/angular-ui-bootstrap/ui-bootstrap-tpls-0.12.1.min'
    },
    shim : {
        // 表明该模块依赖angular
        'angularAMD' : [ 'angular'],
        'angular-route' : [ 'angular'],
        'ui.route':['angular'],
        'ueditorConfig' : ['jquery'],
        'ueditorAll' : ['jquery'],
        'wdatePicker' : ['jquery'],
        'ui-bootstrap' : [ 'angular'],
        'blockUI' : [ 'angular'],
        'angular-sanitize' : [ 'angular' ]
         
    },
    // 启动程序 js/scripts/app.js
    deps : [ 'app' ]
```

#### myapp的代码(定义了一个angularjs自己的模块 `myApp`)

```
//Angularjs会在全局域中添加一个名为 angular 的变量。我们必须在 shim 中显式把它暴露出来，才能通过模块注入的方式使用它
define(['angular'], function(angular) {
angular.module('myapp', [])
      .controller('MyController', ['$scope', function ($scope) {
        $scope.name = 'Change the name';
      }]);

});
```

### index代码

```
<!DOCTYPE html>
<html ng-app="myapp">
<head>
<meta charset="utf-8">
	<title></title>
	<script src="js/lib/requirejs/require.js" data-main="config.js"></script>
</head>
<body >
<div>Hello, world!</div>
<div ng-controller="MyController">
 	<p>名字 : <input type="text" ng-model="name"></p>
 	<h1>{{name}}</h1>
</div>
</body>
</html>
```

[^shim]: shim功能是require中属性,shim块实现让RequireJS加载不兼容AMD的脚本,譬如angular
[^AMD]: 异步模块定义（AMD）的编程接口提供了定义模块，及异步加载该模块的依赖的机制。它非常适合于使用于浏览器环境，浏览器的同步加载模块机制会带来性能，可用性，调试和跨域访问的问题。

#### 注意 require，config，app.js引入层级

config.js必须放在所有js全局

​	<script type="text/javascript" src="mainjs/commonjs/download/requirejs/require.js" data-main="mainjs/config.js""></script>

![](http://i1.piimg.com/567571/51ea0264c89e1f16.png)

###### #参考链接http://www.requirejs.cn/

http://www.runoob.com/w3cnote/requirejs-tutorial-1.html

###### http://www.tuicool.com/articles/ENbI7j3

http://www.cnblogs.com/xiao-yang1990/p/4925297.html