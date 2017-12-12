# ocLazyLoad

### 1. 使用的必要性

随着项目开发引入的模块或文件会逐渐增加，每个路由页面往往不需要其他页面的很多文件加载进来，因此采用ocLazyLoad实现按需加载，提高网页访问的速度，并且改善目录结构

### 2.安装oclazyload

```
bower install oclazyload --save
```

### 3.依赖注入

```
angular.module('test-module', [ 'oc.lazyLoad']);
在ejt项目中，app.module.js引入该模块
```

### 4.应用

在项目中当你切换至某个路由时需要到哪些控制器和service或者第三方组件时，在路由resolve中配置懒加载文件,

```
resolve:object,将会被注入controller去执行的函数
```



以main目录下accountManage下的accountManage.state.js为例（name填加载的文件的模块名称，files中是需要加载的文件数组）

```javascript
                loadPlugin: function ($ocLazyLoad) {
                    return $ocLazyLoad.load([
                        {
                            serie: true,
                            name: 'ngFileUpload',
                            files: ['bower_components/ng-file-upload/ng-file-upload-shim.js','bower_components/ng-file-upload/ng-file-upload.js','content/ui-select.css','bower_components/angular-ui-select/dist/select.js','app/main/accountManage/commitUserAudit.service.js','app/main/accountManage/fullUserinfo.controller.js']
                        }
                    ]);
                },
```

### 5.ocLazyLoad的参数详解：

| Parameter      | Type    | Default value |
| -------------- | ------- | ------------- |
| `cache`        | Boolean | true          |
| `reconfig`     | Boolean | false         |
| `rerun`        | Boolean | false         |
| `serie`        | Boolean | false         |
| `insertBefore` | Boolean | *undefined*   |

cache:false所有懒加载的文件会被浏览器默认缓存

默认情况下，本地加载程序将并行加载文件。如果您需要在serie中加载文件，可以使用`serie: true`：

```
$ocLazyLoad.load（{ serie：true，files：[ 'bower_components / angular-strap / dist / angular-strap.js'，'bower_components / angular-strap / dist / angular-strap.tpl.js' ] } ）;

```

 详情参考官网



参考链接：https://oclazyload.readme.io/