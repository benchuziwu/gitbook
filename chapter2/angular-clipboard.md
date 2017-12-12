# angular-clipboard

将页面上的某段文字或数字复制至剪切板

1.bower install ngclipboard --save

2.引入

```
'bower_components/clipboard/dist/clipboard.js',
'bower_components/ngclipboard/dist/ngclipboard.js'
```

3.模块 注入ngclipboard

```
var myapp = angular.module('myapp', ['ngSanitize', 'ngclipboard'])
```

4.应用

```
<input type="text" id="testCopy" ng-model="test" value="readonly" readonly />
<button class="btn btn-xs" ngclipboard data-clipboard-target="#testCopy">
<i class="fa fa-file" title="复制到剪切板"></i>{{'copy'|translate}}
</button>
```

