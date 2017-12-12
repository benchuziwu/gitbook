# controllerAs,this和$scope的区别

AngularJS 1.2版本中提供了Controller As语法，简单说就是可以在Controller中使用this来替代$scope，使得Controller更像一个传统的JS类，相对于$scope的继承树要理解上要简单一些。

```html
1)$scope 只需要在注入中声明，后面就可以直接在附加数据对象：
controller:
function ACtrl($scope) {
$scope.test = "一个例子"; //在$scope对象中加入test
}
html:
<div ng-controller="ACtrl">
{{test}}
</div>

2) this 则采用了controller as(需要版本为ng 1.2+)写法：
controller:
function BCtrl() {
var vm = this;
this.test = "一个例子"; //在this对象中加入test
}
html:
<!-- vm为自己为当前控制器作的一个简略记号，也可以写作 BCtrl as b,
后面变量便可以在b中引出 如b.test -->
<div ng-controller="BCtrl as vm">
{{vm.test}}
</div>
```

##### 作用范围：

```javascript
1) $scope 中的变量或数据对象我们可以全部拿到，并且上级控制器中的变量也可以在下级控制器中被获取到：
controller:
function ParentCtrl($scope) {
$scope.test = "测试";
$scope.cover ="覆盖测试";
}
function ChildCtrl($scope) {
$scope.cover ="子覆盖测试";
var test = $scope.test; //“测试”
}
html:
<div ng-controller="ParentCtrl">
<p>Parent-test : {{test}}</p>
<p>Parent-cover : {{cover}}</p>
<div ng-controller="ChildCtrl">
<p>Child-test : {{test}}</p>
<p>Child-cover : {{cover}}</p>
</div>
</div>
2) this 中的变量则只适用于当前控制器：
controller:

function ParentCtrl($scope) {
var vm = this;

vm.test = "测试";
vm.cover ="覆盖测试";
}
function ChildCtrl($scope) {
var vm = this;

vm.cover ="子覆盖测试";
}
html:

<div ng-controller="ParentCtrl as parent">
<p>Parent-test : {{parent.test}}</p>
<p>Parent-cover : {{parent.cover}}</p>
<div ng-controller="ChildCtrl as child">
<p>Child-test : {{child.test}}</p>
<p>Child-cover : {{child.cover}}</p>
</div>
<div ng-controller="ChildCtrl as parent">
<p>Child-test : {{parent.test}}</p>
<p>Child-cover : {{parent.cover}}</p>
</div>
</div>
在使用this的时候，各层级变量的命名空间是平行的状态，模板html中只可以拿到当前控制器下声明的变量。
```

##### 对象对比：

```javascript
controller:
function CCtrl($scope) {
var vm = this;
$scope.logThisAndScope = function() {
console.log(vm === $scope)
}
}
vm与$scope实际上是不相等的，在console中我们可以看到
vm: Constructor;
$scope: $get.Scope.$new.Child;
而在$scope中又包含了一个变量vm: Constructor
```

实际结构

```
$scope: {


...,


vm: Constructor,


...


}
```

$scope 当控制器在写法上形成父子级关系时，子级没有的变量或方法父级会自动强加在子级身上，子级可以任意获取到当前父级的变量或方法，该种形式是不可逆的，即父级无法通过$scope获取到子级的任意变量或方法。this 则像一个独立的个体，所有东西都是由自己管理，也就不存在父子级变量混淆关系了。

参考链接：http://stackoverflow.com/questions/11605917/this-vs-scope-in-angularjs-controllers

[http://codetunnel.io/angularjs-controller-as-or-scope/](https://www.gitbook.com/book/dandelion1000/front-end-road/edit#)

