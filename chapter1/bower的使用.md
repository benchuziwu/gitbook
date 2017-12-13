# bower 配置过程

## 建立bower的运行环境配置

``` mkdir bowerprojecttest
mkdir bowerprojecttest
cd bowerprojecttest
npm install -g bower
touch  .bowerrc//touch创建文件  更改使用bower下载安装js包时的路径,{"directory":"js/lib"}譬如使用bower安装angular后去文件夹该目录下寻找
cat .bowerrc 查看
bower init //根据提示初始化生成一个bower.json文件
bower install PACKAGE --save//save new dependencies to your bower.json with bower install PACKAGE --save bower管理js包依赖
```

##### 关于bower install PACKAGE后面是否添加—save的区别

```
(以安装angular为例)
##添加－－save来安装包时全局下bower.json就会管理js包的依赖
    bower.json中dependencies是这样滴
      "dependencies": {
        "requirejs": "^2.3.2",
        "angular": "^1.5.8"
      }
##若不添加－－save则只下载包，而bower.json中就不会出现此包的管理依赖
    bower.json中dependencies是这样滴
        "dependencies": {
        "requirejs": "^2.3.2"
      }
  
```

选择插件