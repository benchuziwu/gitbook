项目中会使用到对于表单元素的一些校验，AngularJS 表单和控件可以提供验证功能，并对用户输入的非法数据进行警告。

使用angularjs对于表单元素的双向绑定校验，可以有效减少校验代码

HTML 表单属性 **novalidate** 用于禁用浏览器默认的验证。

###### #以验证用户名和邮箱为例

(可以使用bootstrap的alert系列样式，然后调整一下alert的展示位置即可)

```html
<form ng-app="myApp" ng-controller="validateCtrl"
name="myForm" novalidate>
<p>用户名:<br>
<input type="text" name="user" ng-model="user" required>
<span class="alert alert-danger" ng-show="myForm.user.$dirty && myForm.user.$invalid">
<span ng-show="myForm.user.$error.required">用户名是必须的。</span>
</span>
</p>
<p>邮箱:<br>
<input type="email" name="email" ng-model="email" required>
<span class="alert alert-danger" ng-show="myForm.email.$dirty && myForm.email.$invalid">
<span ng-show="myForm.email.$error.required">邮箱是必须的。</span>
<span ng-show="myForm.email.$error.email">非法的邮箱地址。</span>
</span>
</p>
<p>
<input type="submit"
ng-disabled="myForm.user.$dirty && myForm.user.$invalid ||
myForm.email.$dirty && myForm.email.$invalid">
</p>
</form>

<script>
var app = angular.module('myApp', []);
app.controller('validateCtrl', function($scope) {
$scope.user = 'John Doe';
$scope.email = 'john.doe@gmail.com';
});
</script>
```

| 属性        | 描述       |
| --------- | -------- |
| $dirty    | 表单有填写记录  |
| $valid    | 字段内容合法的  |
| $invalid  | 字段内容是非法的 |
| $pristine | 表单没有填写记录 |