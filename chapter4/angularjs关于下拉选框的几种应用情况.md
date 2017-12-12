项目中会经常用到下拉选框的情形：

###### 1.默认选中某一个值

```Html
<div ng-controller="ExampleController">
  <form name="myForm">
    <label for="mySelect">Make a choice:</label>
    <select name="mySelect" id="mySelect"
      ng-options="option.name for option in data.availableOptions track by option.id"
      ng-model="data.selectedOption"></select>
  </form>
  <hr>
  <tt>option = {{data.selectedOption}}</tt><br/>
</div>
```

````javascript
angular.module('defaultValueSelect', [])
 .controller('ExampleController', ['$scope', function($scope) {
   $scope.data = {
    availableOptions: [
      {id: '1', name: 'Option A'},
      {id: '2', name: 'Option B'},
      {id: '3', name: 'Option C'}
    ],
    selectedOption: {id: '3', name: 'Option C'} //This sets the default value of the select in the ui
    };
}]);
````

###### 2.不默认选中

```html
<div ng-controller="ExampleController">
  <form name="myForm">
    <label for="repeatSelect"> Repeat select: </label>
    <select name="repeatSelect" id="repeatSelect" ng-model="data.model">
            <option >请选择一个</option>
      <option ng-repeat="option in data.availableOptions" value="{{option.id}}">{{option.name}}</option>
    </select>
  </form>
  <hr>
  <tt>model = {{data.model}}</tt><br/>
</div>
```

3.不用ng-repeat

```
    <select name="singleSelect" ng-model="data.singleSelect">
      <option value="option-1">Option 1</option>
      <option value="option-2">Option 2</option>
    </select><br>
```



