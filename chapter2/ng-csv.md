# ng-csv

系统中涉及到需要导出excel表格时，使用ng-csv功能导出数据表格

### 1.bower安装ng-csv

```
bower install ng-csv --save
```

### 2.  引入该插件

```javascript
<scrpit src="bower_components/ng-csv/build/ng-csv.js"></script>
```

### 3.依赖注入

```
var myapp = angular.module('testapp', ['ngSanitize', 'ngCsv'])
```

### 4.添加指令到页面的导出excel按钮

```html
<button type="button" ng-csv="getArray" filename="test.csv">导出</button>

<button type="button" ng-csv="getArray" csv-header="['Field A', 'Field B', 'Field C']" filename="test.csv">导出</button>
```

###### getArray是你需要导出的数据，可以是数组，一个值，一个表达式，filename以及需要导出的内容可根据具体情况进行配置命名，csv-header设置需要展现数据的表头的属性

在ejt项目中有个管理员查看系统套餐包页面，有个需要导出excel测功能，是这样使用的

```html
<button class="btn btn-primary ng-isolate-scope btn-sm" ng-csv="getSkuPackageArrays" filename="{{filename}}.csv" csv-header="['ID','优先级','套餐包类型','最大条数','最小条数','单价','创建时间','创建人','标识']" add-bom="true" >{{'commonOperate.Excel'|translate}}</button>
```

add-bom="true"加上以后可以防止导出的表格出现乱码

getSkuPackageArrays是你需要导出excel的数据对象

参考链接：https://github.com/asafdav/ng-csv

https://asafdav.github.io/ng-csv/example/

[http://stackoverflow.com/questions/27041800/angularjs-ng-csv-files-not-downloading](https://www.gitbook.com/book/dandelion1000/front-end-road/edit#)

