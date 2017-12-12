angular-ui-select提供typehead的功能，提前获取所有数据并且将其做成下拉选框功能

  学习链接http://angular-ui.github.io/ui-select/#examples

1.单选

```html
  <ui-select ng-model="formData.dataobj" theme="bootstrap">
         <ui-select-match ng-attr-placeholder="{{'test.placeholder1' | translate}}">{{$select.selected.value}}</ui-select-match>
         <ui-select-choices repeat="item in allDatas | filter: $select.search"><div ng-bind-html="item.value | highlight: $select.search"></div></ui-select-choices>
</ui-select>
```

allDatas是获取所有数据的列表，item.value是数据数组的每一个对象的属性之一，用于显示在下拉选框中

2.  多选

```html
   <ui-select multiple ng-model="formData.selected" theme="bootstrap" sortable="true" close-on-select="false">
   <ui-select-match ng-attr-placeholder="{{'test.placeholder1' | translate}}">{{$item.name}</ui-select-match>
     <ui-select-choices repeat="item in allDatas | filter: $select.search"><div ng-bind-html="item.name | highlight: $select.search"></div>
     </ui-select-choices>
   </ui-select>
```

   ​