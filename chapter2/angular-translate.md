# angular-translate

项目中使用多语言环境对于不同语言种族的用户来说可以提高用户体验。

使用angular-translate插件作为前端翻译解决方案之一

### 1.翻译模块的基本使用

##### （1）安装

```
bower install angular-translate --save
bower install angular-translate-loader-static-files --save
```

 angular-translate是angular官方提供的国际化模块

 angular-translate-loader-static-files.min.js 模块是用来读取本地文件的模块,因为我们的翻译内容都是独立的 json 文件.

##### （2）运用过程

建一个独立的文件夹 i18n 用来放json文件,目录及文件如下层级关系:

/i18n/

        en.json

        cn.json

en.json 文件内容如下:

```
{"title":"test","content":"testContent"}
```

cn.json 文件内容如下:

```
{"title":"测试","content":"测试内容"}
```

注入依赖

```
var app = angular.module('myApp', ['pascalprecht.translate'])
.config(['$translateProvider',function($translateProvider){
        var lang = window.localStorage.lang||'cn';
    $translateProvider.preferredLanguage(lang);
    $translateProvider.useStaticFilesLoader({
        prefix: '/i18n/',
        suffix: '.json'
    });
}]);
localStorage.lang  来存储用户上一次选择的语言,如果用户是第一次翻译,默认显示中文(及 加载 cn.json 文件来翻译)
$translateProvider.preferredLanguage(lang)
这一句告诉 angular.js 哪种语言是默认已注册的语言.
 $translateProvider.useStaticFilesLoader({
        prefix: '/i18n/',
        suffix: '.json'
    }
告诉angular.js 应该加载本地那些国际化语言配置文件.
prefix : 指定文件前缀.
suffix: 指定文件后缀.


```

在用户页面左侧或右侧放置选择语言的下拉列表框

```
<div class="col-sm-2">
<a class="col-sm-2" ng-click="changeLanguage('cn1')" translate="BUTTON_LANG_ZH"></a>
<a class="col-sm-2" ng-click="changeLanguage('en1')" translate="BUTTON_LANG_EN"></a>
</div>
```

```
angular.module('testAPP').controller('loginCtrl',function($scope,$translate){
        $scope.changeLanguage = function (langKey) {
         $translate.use(langKey);
    }; 
    })
```

```
<span data-translate="title1"></span>
或者<span>{{'title1' | translate}}</span>
placeholeder的翻译写法ng-attr-placeholder="{{'placeholder' | translate}}"
<input type="submit" class="btn btn-primary  block full-width m-b"  translate  translate-attr-value="{{'submit' | translate}}"
```





