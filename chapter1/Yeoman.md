# Yeoman

—一个用于构建特定框架的生态系统的代码工具

###用yeoman初始化一个基本的angularjs框架应用

前言：关于安装yo的坑，新建文件夹执行一系列命令之后，发现目标文件夹下没有任何内容；该如何解决？应执行操作如下：

```
cd 命令回到根目录
ls -a查看根目录下所有文件
找到.yo-rc.json这个文件
执行sudo rm -f .yo-rc.json
然后重新按照大体步骤执行即可
```

####安装Yo 

```
npm install -g yo//全局安装
yo --version//查看版本
npm install -g yo gulp bower//先安装依赖的gulp和bower
```

```
 mkdir yeomantestangular
 cd yeomantestangular
```

#### 先安装生成器

```
npm install -g generator-karma generator-angular
```

### 整合一下初始命令

```
npm install -g yo gulp bower generator-karma generator-angular
```

##### 安装完毕后

├── bower@1.7.9
├── generator-angular@0.15.1
├── generator-karma@2.0.0
├── gulp@3.9.1
└── yo@1.8.5

#### 接下来执行yo命令创建angular项目

```
yo angular [modulename]//[]内写module的名称
yo angular xianxiantestyeoman
 Would you like to use Gulp (experimental) instead of Grunt? Yes
 Would you like to use Sass? No
? Would you like to include Bootstrap? Yes
接下来都是自动执行，需等待
最后执行 gulp serve就打开示例demo了
```

至此打开目标文件夹一个自动化种子项目就生成了