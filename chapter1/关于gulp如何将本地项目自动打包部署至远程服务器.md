### 关于gulp如何将本地项目自动打包部署至远程服务器

```
var gulp = require('gulp'),
sftp=require('gulp-sftp'),//gulp自动远程上传至服务器
gulp.task('autosftp',function(){
  return gulp.src('release/**')
  .pipe(sftp({
        host:'182.92.102.97',
        port:'9393',
        remotePath:'./testejtfront',
        user:'root',
        password:'Zxxydy123'
  }))
})
gulp.task('default',['webserver','jshint','copy-to-one-folder','test','publish','connect','auto','script','autosftp'])

```

![](http://yotuku.cn/link?url=BkDPzyIbe&tk_plan=free&tk_storage=tietuku&tk_vuid=c2bdaf06-0f79-4fc9-8225-17a1ff914a1a&tk_time=2016111320)

