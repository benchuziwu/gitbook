

### 关于angular单元测试

​                                        －－－－－karma与Jasmine测试angulajs 应用

```
karma是自动化测试工具，就是将要测试的js脚本和测试用例以及依赖文件引入，然后就可以使用karma start my.conf.js执行测试过程，用上gulp自动化工具就可以使karma任务自动执行，此时终端输入gulp则一切任务都自动执行啦，而Jasmine是测试框架，专门用来写测试用例的，就是测试用例js是用Jasmine语法(详见Jasmine官网)写的。
```

#### 关于单元测试的重要性和意义

```
1.验证你目前所写的代码有没有问题,是否满足功能性要求（对于自己开发的模块，只有在通过单元测试之后，才能提交到Git 库）。
2.更为重要的是，一次编写好的测试用例是否可以在日后随时随地地运行，来验证你本次所修改的代码是否影响到了以往的业务逻辑。
3.使别人能快速了解业务逻辑。
```

###### 测试驱动开发模式：

```
旨在强调在开发功能代码之前，先编写测试代码。也就是说在明确要开发某个功能后，首先思考如何对这个功能进行测试，并完成测试代码的编写，然后编写相关的代码满足这些测试用例。然后循环进行添加其他功能，直到完成全部功能的开发。
```

#### 如何进行自动化单元测试

​                      －－－－以karma测试angular为例

```
官方网址：http://karma-runner.github.io/1.0/index.html
```

#### 1.安装

```
//安装karma，将会在package.json的devDependencies中看到添加了"karma": "^1.3.0",这行
npm install karma --save-dev
//安装项目所需的的插件
npm install karma-jasmine karma-chrome-launcher --save-dev
npm install -g karma-cli
```

#### 测试运行Karma

```
./node_modules/karma/bin/karma start
```

#### 配置config文件

```
karma init my.conf.js
```

```
Which testing framework do you want to use?
Press tab to list possible options. Enter to move to the next question.
> jasmine

Do you want to use Require.js?
This will add Require.js plugin.
Press tab to list possible options. Enter to move to the next question.
> no

Do you want to capture a browser automatically?
Press tab to list possible options. Enter empty string to move to the next question.
> Chrome
> Firefox
What is the location of your source and test files?
You can use glob patterns, eg. "js/*.js" or "test/**/*Spec.js".
Press Enter to move to the next question.
> *.js
> test/**/*.js  //此处写测试功能用例，，
>
Should any of the files included by the previous patterns be excluded?
You can use glob patterns, eg. "**/*.swp".
Press Enter to move to the next question.
>

Do you want Karma to watch all the files and run the tests on change?
Press tab to list possible options.
> yes

Config file generated at "/Users/vojta/Code/karma/my.conf.js".
```

#### 执行karma测试

```
karma start my.conf.js//根据配置文件开始测试
```

#### 结合gulp配置实现测试更加自动化

```
var gulp = require('gulp'),
webserver = require('gulp-webserver'),
 Server = require('karma').Server;
 gulp.task('test', function (done) {
  new Server({
    configFile: __dirname + '/mykarma.conf.js',
    singleRun: true
  }, done).start();
});
 gulp.task('webserver', function() {
  gulp.src( '.' ) // 服务器目录（.代表根目录）
  .pipe(webserver({ // 运行gulp-webserver
    livereload: true, // 启用LiveReload
    open: true // 服务器启动时自动打开网页
  }));
});
gulp.task('default',['test','webserver']);
执行gulp即可
```

#### 遇到的坑

```
起先iterm终端会报angular is undifined
摸索很久才知道karma配置文件my.conf.js中files第一行少了对angularjs的引入
然后angular is undifined这个问题没有了，又出现module is undifined这个问题
然后在module前加angular此问题又没有了，接下来又出现inject is undifined问题，尝试上面的思路没有用，最终在my.conf.js的files中再引入angular-mocks.js，问题才都解决了
使用bower install angular-mocks －－save安装包依赖
因为karma是测试用例，所以测试我们写的angularjs脚本之前，应该引入angular.js
```

![](http://p1.bpimg.com/567571/186c4e8f97fa8895.png)

#### testapp.js的语法结构按Jasmine写

```
//jasmine的官网http://jasmine.github.io/edge/introduction.html
testapp.js

//var app=angular.module('myapp',[]);
describe('mainCtrl',function(){
	   var scope;
    //module都是angular.mock.module的缩写
    beforeEach(module('myapp'))
    //insject都是angular.mock.inject的缩写
    beforeEach(inject(function($rootScope,$controller){
        scope = $rootScope.$new();
        $controller('mainCtrl',{$scope:scope});
    }));
    it('case1',function(){
        expect(scope.testkarma).toBe('hahahha');
    });
})//注解:scope.testkarma就是类似功能，此处scope.testkarma是数据绑定的内容
期望这个功能点最后的输出内容是所对应的结果
```

#### 关于Jasmine语法

```
//describe Your Tests
describe("A suite", function() {
  it("contains spec with an expectation", function() {
    expect(true).toBe(true);
  });
});
```

#### 将karma与Jasmine用于项目的单元测试中的坑

##### 全局声明是单独写在app.js文件中的

![](http://7xrn7f.com1.z0.glb.clouddn.com/16-9-29/4945903.jpg)

##### 1.测试控制器

```
此时mainctrl控制器中是对模块的引用，
var myapp=angular.module('inspinia'）

karma的config .js的文件中files开始是这样的
      'commonjs/angular/angular.js',
      'commonjs/angular/angular-mocks.js',
      'js/controller/MainCtrl.js',
      'tests/controller-testmain.js'
      
      但是运行iterm会出现问题－－－[$injector:nomod] module 'inspinia' is not available！you either misspelled the module name or misspelled the module name or
```

![](http://7xrn7f.com1.z0.glb.clouddn.com/16-9-29/34371721.jpg)

```
然后我我在mainctrl里将var myapp=angular.module('inspinia'）改成
var myapp=angular.module('inspinia',[]）此时测试不出问题了，但是页面无法加载，(原因是声明了多次module，多次声明module会导致前边的module定义信息被清空，所以程序就会找不到已定义的组件)
```

```
然后我索性将app.js中的声明迁移到mainctrl
并同时去掉干扰项oc.lazyLoad的注入，(后来才知道它才是罪魁祸首)
var myapp=angular.module('inspinia',['ui.router','ui.bootstrap','pascalprecht.translate']);
然后karma的配置文件改成
files: [
    'commonjs/angular/angular.js',
    'commonjs/angular-ui-router/release/angular-ui-router.js',
    'commonjs/bootstrap/dist/js/ui-bootstrap-tpls-1.1.2.min.js',
    'commonjs/angular-translate/angular-translate.js',
    'commonjs/angular-translate-loader-static-files/angular-translate-loader-static-files.js',
    'commonjs/angular/angular-mocks.js',
    'js/injection/app.js',
    'js/controller/MainCtrl.js',
    'tests/controller-testmain.js'
    ],
然后页面正常运行，测试也成功了，但是这并不是标准的写法,(见下文)
```

###### 关于模块

```
angular.module(‘com.ngbook.demo’, [可选依赖])和angular.module(‘com.ngbook.demo’),注意它是完全不同的方式，一个是声明创建module，而另外一个则是获取已经声明了的module.在应用程序中，对module的声明应该有且只有一次；对于获取module，则可以有多次。推荐将angular组件独立分离在不同的文件中，module文件中声明module，其他组件则引入module，需要注意的是在打包或者script方式引入的时候，我们需要首先加载module声明文件，然后才能加载其他组件模块。
```

```
测试成功之后，把oc.lazyLoad加上，但是会出现问题
```

![](http://7xrn7f.com1.z0.glb.clouddn.com/16-9-29/58341621.jpg)

```
经google搜索，使用bower install oclazyload --save 重新安装了依赖，并修改了karma的配置文件files中关于对oclazyload.js的引用，当初oclazyload我未使用bower install oclazyload --save 安装依赖，而是直接用的原来本身plugins目录下的oclazyload，然后测试通过，接着我把全局模块声明又放到单独app.js文件中，测试，页面都成功啦
```

![](http://7xrn7f.com1.z0.glb.clouddn.com/16-9-29/31315353.jpg)



参考链接：

 单元测试的重要性

http://book.51cto.com/art/201503/467154.htm

http://blog.csdn.net/happylee6688/article/details/37962283

jasmine的官网http://jasmine.github.io/edge/introduction.html

 karma的官方网址：http://karma-runner.github.io/1.0/index.html