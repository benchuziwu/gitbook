# AngularJs $sce

$sce 服务是AngularJs提供的一种严格上下文转义服务。

严格的上下文转义服务

严格的上下文转义(SCE)是一种需要在一定的语境中导致AngularJS绑定值被标记为安全使用语境的模式。由用户通过ng-bind-html绑定任意HTML语句就是这方面的一个例子。我们称这些上下文转义为特权或者SCE。

下面代码是简化了的ngBindHtml实现（当然，这不是完整版ngBindHtml源码）：

```
var ngBindHtmlDirective = ['$sce', function($sce) {
return function(scope, element, attr) {
scope.$watch($sce.parseAsHtml(attr.ngBindHtml), function(value) {
element.html(value || '');
});
};
}];
```

方法：

isEnabled();

返回一个boolean，指示是否可启用SCE。

parseAs(type,expression);

将Angular表达式转换为一个函数。这类似$parse解析并且当表达式是常量时是相同的。否则，它将调用$sce.getTrusted(type,result)将表达式包装。

type：在SCE的上下文的使用的结果的类型。

expression：被编译的字符串表达式。

trustAs(type,value);

代表$sceDelegate.trustAs。

type：上下文中能安全的被使用的值，如url，resourceUrl，html，js和css。

value：需要被认为是安全或者值的信赖的值。

trustAsHtml(value);

$sceDelegate.trustAs($sce.HTML,value)的快捷方式。

value：被信任的值。

trustAsUrl(value);

$sceDelegate.trustAs($sce.URL,value)的快捷方式。

value：被信任的值。

trustAsResourceUrl(value);

$sceDelegate.trustAs($sce.RESOURCE_URL,value)的快捷方式。

value：被信任的值。

trustAsJs(value);

$sceDelegate.trustAs($sce.JS,value)的快捷方式。

value：被信任的值。

getTrusted(type,maybeTrusted);

代表$sceDelegate.getTrusted。因此，得到了$sce的结果。如果查询的上下文类型是一个创造型的类型，则调用trustAs()并且返回原来提供的值。如果这个条件不满足，则抛出一个异常。

getTrustedHtml(value);

$sceDelegate.getTrusted ($sce.HTML,value)的快捷方式。

value：通过$sce.getTrusted执行后的值。

getTrustedCss(value);

$sceDelegate.getTrusted ($sce.CSS,value)的快捷方式。

value：通过$sce.getTrusted执行后的值。

getTrustedUrl(value);

$sceDelegate.getTrusted ($sce.URL,value)的快捷方式。

value：通过$sce.getTrusted执行后的值。

getTrustedResourceUrl(value);

$sceDelegate.getTrusted ($sce.RESOURCE_URL,value)的快捷方式。

value：通过$sce.getTrusted执行后的值。

getTrustedJs(value);

$sceDelegate.getTrusted ($sce.JS,value)的快捷方式。

value：通过$sce.getTrusted执行后的值。

parseAsHtml(expression);

$sce.parseAs ($sce.HTML,value)的快捷方式。

value：被编译的字符串表达式。

parseAsCss(expression);

$sce.parseAs ($sce.CSS,value)的快捷方式。

value：被编译的字符串表达式。

parseAsUrl(expression);

$sce.parseAs ($sce.URL,value)的快捷方式。

value：被编译的字符串表达式。

parseAsResourceUrl(expression);

$sce.parseAs ($sce.RESOURCE_URL,value)的快捷方式。

value：被编译的字符串表达式。

parseAsJs(expression);

$sce.parseAs ($sce.JS,value)的快捷方式。

value：被编译的字符串表达式。

使用方式：

```html
<div ng-app="Demo" ng-controller="testCtrl as ctrl">
<textarea ng-model="ctrl.jsContext"></textarea>
<pre ng-bind="ctrl.jsBody"></pre>
<button ng-click="ctrl.runJs()">Run</button>
<div ng-bind-html="ctrl.htmlText" class="myCss"></div>
</div>
```

```javascript
(
function () {
angular.module('Demo', [])
.controller('testCtrl', ["$sce","$scope",testCtrl]);
function testCtrl($sce,$scope) {
var vm = this;
$scope.$watch("ctrl.jsContext",function(n){
vm.jsBody = n;
});
this.runJs = function() {
eval(vm.jsBody.toString());
};
vm.htmlText = "<h2>Hello World</h2>";
vm.htmlText =$sce.trustAsHtml(this.htmlText);
}
}());
```

上面是一个将不被Angular认定为信任的HTML代码字符串通过$sce设置为信任的HTML代码并且插入的例子，这里用了个小技巧，也就没在controller进行这步操作了，直接放到一个filter服务内，只要在需要的地方过滤下即可，并且可指定类型，这里写成统一动态选择类型了。那么在啥时候需要用到这两个服务呢？在当使用ng-bind-html绑定html时报错：Error: [$sce:unsafe]Attempting to use an unsafe value in a safe context. 的时候使用。

###### 补充在iframe中嵌入一段html标签

```
angular
.module('ejt1GatewayApp').filter('trustHtml', function ($sce) {
return function (input) {
return $sce.trustAsHtml(input);
}
});
```

```
<iframe id="alipayiframe" style="width:590px;height:360px" srcdoc="{{form|trustHtml}}" frameborder="0">
</iframe>
```

form为一段html标签