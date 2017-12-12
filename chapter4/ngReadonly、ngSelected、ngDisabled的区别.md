#### ngReadonly

该指令将input，textarea等文本输入设置为只读。HTML规范不允许浏览器保存类似readonly的布尔值属性。如果我们将一个Angular的插入值表达式转换为这样的属性，那么当浏览器删除该属性时，绑定信息就会丢失。这个指令不被浏览器删除，并且提供了一个永久性的可靠的地方来存储绑定信息。

**格式**：ng-readonly=“value”value：表达式 结果为boolean类型使用代码：

```html
<input type="checkbox" ng-model="checked"><br />
<input type="text" ng-readonly="checked" value="Hello World" />
```

这个指令用的比较多的地方如编辑资料，但是某些字段是只读的，不让编辑的，这时候就可以设置ngReadonly=“true”了，并且还是可通过js直接操作，只需要操作的表达式返回值是true/false即可。然后在项目中用到大多在datetimepicker插件上设置这个指令，只能通过日历选择日期，而不能直接输入来选择。

#### ngSelected

该指令为select设置了指定的选中值。

HTML规范不允许浏览器保存类似selected的布尔值属性。如果我们将一个Angular的插入值表达式转换为这样的属性，那么当浏览器删除该属性时，绑定信息就会丢失。这个指令不被浏览器删除，并且提供了一个永久性的可靠的地方来存储绑定信息。

**格式**:ng-selected=“value”

value：表达式 结果是boolean类型。

使用代码：

```
<input ng-model="checked" type="checkbox" />
<select>
<option>Hello</option>
<option ng-selected="checked">World</option>
</select>
```

```
<div ng-app="Demo" ng-controller="testCtrl as ctrl">
<select ng-model="ctrl.getId" ng-options="i.id as i.value for i in ctrl.list">
</select>
</div>
```

```
(
function () {
angular.module("Demo", [])
.controller("testCtrl", testCtrl);
function testCtrl() {
this.list = [{ id: 1, value: "获取1的id" }, { id: 2, value: "获取2的id" }, { id: 3, value: "获取3的id" }];
this.getId = 2;
};
}());
```

#### ngDisabled

该指令在chrome，firefox的button启用起效，在ie8及以下版本ie浏览器无效。

HTML规范不允许浏览器保存类似selected的布尔值属性。如果我们将一个Angular的插入值表达式转换为这样的属性，那么当浏览器删除该属性时，绑定信息就会丢失。这个指令不被浏览器删除，并且提供了一个永久性的可靠的地方来存储绑定信息。

格式：ng-disabled=”value”

value: boolean类型 判断所在标签是否可用。

参考 链接：[https://docs.angularjs.org/api/ng/directive/select](https://www.gitbook.com/book/dandelion1000/front-end-road/edit#)

