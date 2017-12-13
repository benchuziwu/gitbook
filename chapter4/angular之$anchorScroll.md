# angular $anchorScroll链接至本页某处

##### $anchorScroll

根据HTML5的规则，当调用这个函数时，它检查当前的url的hash值并且滚动到相应的元素。

监听$location.hash()并且滚动到url指定的锚点的地方。

可以通过$anchorScrollProvider.disableAutoScrolling()禁用。

**依赖**：$window $location $rootScope

**使用**：$anchorScroll()

```html
<div ng-app="Demo" ng-controller="testCtrl as ctrl">
<div id="top" ng-click="ctrl.gotoBottom()">跳到底部</div>
<div id="bottom" ng-click="ctrl.gotoTop()">跳到顶部</div>
</div>
```

```javascript
(
function () {
angular.module("Demo", [])
.controller("testCtrl",["$location", "$anchorScroll",testCtrl]);
function testCtrl($location,$anchorScroll){
this.gotoTop = function () {
$location.hash("top");
$anchorScroll();
};
this.gotoBottom = function () {
$location.hash("bottom");
$anchorScroll();
};
};
}());
```

参考链接：https://docs.angularjs.org/api/ng/service/$anchorScroll

