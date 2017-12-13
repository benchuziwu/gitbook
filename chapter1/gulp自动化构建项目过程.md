[TOC]

##gulp自动化构建项目过程

### gulp介绍

1.css压缩 　　gulp-clean-css

2.js压缩　　　gulp-uglify

3.js合并　　　gulp-concat　　

gulp借鉴了Unix操作系统的管道（pipe）思想，前一级的输出，直接变成后一级的输入，gulp.src返回当前文件流至可用插件;异步任务支持；

压缩 js 代码可降低 js 文件大小，提高页面打开速度;gulp通过代码指令即可压缩

gulp是基于nodejs的自动任务运行器，她能自动地完成 文件的测试，检查，合并，压缩，格式化，浏览器自动刷新，部署文件生成，并监听文件在改动后重复指定的这些步骤

```
gulp
```

###npm介绍

```
安装完node以后，npm自动安装成功；
npm init初始化一个package.json文件
```

### 以bower＋gulp为例的js  DEMO

####新建文件夹测试gulpdemo

```
mkdir testgulp
```

####初始环境搭建：安装gulp

#####全局安装

```
npm install -g gulp//全局安装gulp，-g表示在全局环境安装，以便任何项目都能使用它
```

##### 将gulp安装到项目本地

```
npm install —-save-dev gulp //—-save-dev 来更新package.json文件,更新 devDependencies 值，以表明项目需要依赖gulp。Dependencies 可以向其他参与项目的人指明项目在开发环境和生产环境中的node模块依赖关系
```

####查看gulp的版本

```
gulp -v
```

#### 创建**gulpfile.js**

#####1.获取 gulp

```
var gulp = require('gulp');
```

#####获取 uglify 模块（用于压缩 JS)

```
var uglify = require('gulp-uglify');
```

#####压缩 js 文件 

```
.pipe(uglify())
```

#####创建压缩任务

```
gulp.task('script', function() {
    gulp.src('js/myapp.js')// 1. 找到目标压缩文件位置
    .pipe(uglify())      // 2. 压缩文件
    .pipe(gulp.dest('dist/js/myjavascript')) // 3. 另存压缩后的文件，如果该文件夹不存在，将会自动创建它，执行gulp script命令后将可在根目录下看到dist／js/myjavascript文件夹并且有压缩后的myapp.js
});
在命令行使用gulp script启动此任务
```

##### 关于gulpfile.js的注释

```
gulp.task(taskname, function(){taskContent}) 
gulp.src(filepath) 
gulp.dest(filepath)
gulp.pipe() //管道,pipe可理解为将操作加入执行队列
```

#### gulp的watch用法

```
监视文件，并且可以在文件发生改动时候做一些事情
```

###### 举例

```
gulp.watch('js/myapp.js',['script'])//监测myapp.js，譬如改动就执行任务script
```

#### gulp的task(scrpit)功能

```
／／script表示js文件，如果在此创建压缩任务，
gulp.task('script', function(){}
```

注意：gulp.src('js/＊.js')

```
*表示所有的意思，这样一来就可以同时压缩多个js文件
```



#### gulp的task（auto）功能

```
在命令行输入 gulp auto，自动监听 js/*.js 文件的修改后压缩js
```

###### 举例

```
gulp.task('auto',function(){
	gulp.watch('js/myapp.js',['script'])
})
```

####使用 gulp.task('default', fn) 定义默认任务

```
gulp.task('default', ['script', 'auto']);
```

#### 关于task(任务)之间的相互依赖

```
定义一个所依赖的 task 必须在这个 task 执行之前完成
```

###### 示例代码

```
var gulp = require('gulp');
var uglify = require('gulp-uglify');
gulp.task('script', function() {
    gulp.src('js/myapp.js')
        .pipe(uglify())
        .pipe(gulp.dest('dist/js/myjavascript'))
});
gulp.task('config',function(){
	    gulp.src('config.js')
        .pipe(uglify())
        .pipe(gulp.dest('dist/js'))
})
gulp.task('auto',function(){
	gulp.watch('js/myapp.js',['script'])
})
gulp.task('default',['script','auto'])
```

### gulpfile 的 task 依赖树

![](http://i1.piimg.com/567571/2ef2021c1d4ee94e.png)

#### 同时输出一个压缩过和一个未压缩版本的文件

```
//gulp-rename 然后 pipe 到 dest 两次来实现
var gulp = require('gulp');
var rename=require('gulp-rename');
var DEST="build/";
var uglify = require('gulp-uglify');
gulp.task('script', function() {
    gulp.src('js/myapp.js')
    .pipe(gulp.dest(DEST))
        .pipe(uglify())
        .pipe(rename({extname:'.min.js'}))
        .pipe(gulp.dest(DEST))
});
```

#### 使用Gulp实现自动监测页面刷新

###### Gulp-connect;通过watch任务和livereload设置实现自动刷新的： 

```
npm install -g gulp-connect  
```

```
// 实时更新浏览器界面
gulp.task('watch', function () {  
    gulp.watch(['views/main.html'], ['html']);  
});  
//使用connect启动一个Web服务器 
var
livereload = require('gulp-livereload'), 
connect = require('gulp-connect');
gulp.task('connect', function () {  
    connect.server({  
        livereload: true  
    });  
});  
gulp.task('html', function () {  
    gulp.src('views/main.html')  
        .pipe(connect.reload());  
});  
gulp.task('auto',function(){
	gulp.watch('js/*/*.js',['script']);
	gulp.watch(['views/main.html'], ['html']);
	gulp.watch('css/*.css',['css'])
})
gulp.task('default',['webserver','connect','auto','script','css'])

```

#### 使用Gulp实现执行gulp命令即自动加载打开页面

```
var webserver = require('gulp-webserver'),
//需要执行npm install gulp-webserver命令
gulp.task('webserver', function() {
  gulp.src( '.' ) // 服务器目录（.代表根目录）
  .pipe(webserver({ // 运行gulp-webserver
    livereload: true, // 启用LiveReload
    open: true // 服务器启动时自动打开网页
  }));
});
```

### js合并 gulp-concat

```
concat = require('gulp-concat')，
.pipe(concat('main.js'))  
gulp.task('script', function() {
    // 1. 找到文件
    gulp.src('js/*/*.js')
    //.pipe(gulp.dest(DEST))
       // 2. 压缩文件
        .pipe(uglify())
        .pipe(rename({extname:'.min.js'}))
        .pipe(concat('all.min.js')) 
        .pipe(gulp.dest(DEST))
    // 3. 另存压缩后的文件    
});
```

#### 把需要发布的从多处汇总到一个目录dist/src

```
把需要发布的从多处汇总到一个目录dist/src
gulp.task("copy-to-one-folder", function(){
gulp.src(["c:/labs3/TestLab/NodeServer/routes/**/*"], {base: "c:/labs3/TestLab/"})
.pipe(gulp.dest('dist/src'));

return gulp.src(["c:/labs3/TestLab/HomeFinder/**/*"], {base:"c:/labs3/TestLab/HomeFinder/"})
.pipe(gulp.dest('dist/src'));
});
```

#### gulp task之runSequence

```
任务的顺序用runSequence来组织，把可以并行的的任务放在一个数组里面， 数组之间是串行的 
runSequence = require('run-sequence')
runSequence(['clean'], ['copy-to-one-folder'], ['publish'], callback);

(备注： 加形参callback彻底变成串行)
```



#### 如何自动化改变项目的版本号

```
gulp.task('bump-version', function (){})
gulp.task('commit-changes', function () {})
gulp.task('push-changes', function (cb) {
  git.push('origin', 'master', cb);
})
  function getPackageJsonVersion () {
    // 这里我们直接解析 json 文件而不是使用 require，这是因为 require 会缓存多次调用，这会导致版本号不会被更新掉
    return JSON.parse(fs.readFileSync('./package.json', 'utf8')).version;
  };
});
gulp.task('release', function (callback) {})
```



参考链接：http://wiki.jikexueyuan.com/project/gulp-book/chapter2.html

​         http://www.gulpjs.com.cn/docs/

http://www.tuicool.com/articles/nAzquyE

http://www.tuicool.com/articles/IZR3AfV

http://www.cnblogs.com/GameEngine/p/5229576.html



