# angular-growl

用于提示，该插件提供多样式的提示效果

![](http://i1.piimg.com/567571/3d3b4289d3e64999.png)

使用步骤

1.安装

```
bower install angular-growl --save
```

2.引入

```
<link href="angular-growl.css" rel="stylesheet">
<script src="angular-growl.js"></script>
```

3.依赖注入

```
angular.module('testApp', ['angular-growl'])
```

4.运用

控制器中注入growl

配置growl

```
app.controller("demoCtrl", ['$scope', 'growl', function($scope, growl) { 
$scope.addSpecialWarnMessage = function() {
growl.warning("This adds a warn message"); 
growl.info("This adds a info message"); 
growl.success("This adds a success message"); 
growl.error("This adds a error message"); }}]);
```

```
//给提示设置时间倒计时
app.config(['growlProvider', function(growlProvider) {
  growlProvider.globalTimeToLive({success: 1000, error: 2000, warning: 3000, info: 4000});
}]);
```

在ejt项目中主要根据不同的情况进行提示

当输入手机号不合法时有个提示,config可用来配置该提示显示多久

```
var config = {ttl: 400000};
growl.error("<span>errorMessage</span>", config);
```

5.补充关于angular-growl如何做翻译，只要项目中用了angular-translate做翻译，angular-growl会自动将消息翻译，只要当前的消息文本使用了翻译过滤器。

示例：

```
growl.error("<span>{{'Register-html.formcontent.loginExist' | translate}}</span>", config);
```

