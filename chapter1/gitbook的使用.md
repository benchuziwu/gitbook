# gitbook的使用全过程

##### 关于git的安装介绍使用

参考http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000

### 1.gitbook的介绍

```
GitBook 是一个基于 Node.js 的命令行工具，可使用 Github/Git 和 Markdown 来制作精美的电子书;
GitBook.com 为每本书籍都创建了一个 Git 项目，并且使用这个 Git 项目来管理书籍源码
```

### 2.gitbook命令行安装

```
npm install gitbook -g
npm install -g gitbook-cli  
gitbook -V／／查看版本
mkdir testgitbooks
```

### 3.README.md 与 SUMMARY编写

```
README.md和SUMMARY.md 是Gitbook项目必备的两个文件，也就是一本最简单的gitbook也必须含有这两个文件，它们在一本Gitbook中具有不同的用处。
README.md:相当于章节内容；
SUMMARY.md这个文件是一本书的目录结构
```

```
touch README.md
touch SUMMARY.md
```

```
在SUMMARY.md输入
* [简介](README.md)
* [第一章](chapter1/README.md)
 - [第一节](chapter1/section1.md)
 - [第二节](chapter1/section2.md)
* [第二章](chapter2/README.md)
 - [第一节](chapter2/section1.md)
 - [第二节](chapter2/section2.md)
* [结束](end/README.md)
```

### 4.生成图书目录

```
gitbook init
//执行该命令后就会出现如下图所示目录
```

![](http://p1.bpimg.com/567571/a7c4ebb37f1a73e5.png)

```
gitbook给我们生成了与SUMMARY.md所对应的目录及文件。
每个目录中，都有一个README.md文件，相当于一章的说明。
每一章节的文档就由我们需要写什么文档而定了:),写完保存至chapter目录下即可
```

### 5.生成静态网页书籍

```
gitbook serve
```

![](http://i1.piimg.com/567571/f252a25ae5f162d9.png)

```
打开网址http://localhost:4000就可以看到自己写的书了
```

### 6.如何将自己写的markdown文档提交到gitbook上

```
1.git init 
2.git add README.md SUMMARY.md
3.git commit -m "first commit"
4.git remote add gitbook https://git.gitbook.com/dandelion1000/gulp-studying.git
句型：
git remote add gitbook https://git.gitbook.com/{username}/{Book}.git
5.git push -u -f gitbook master
需要提交什么文件依次按2345顺序来即可
```

### 7.补充说明如何将Github仓库与gitbook绑定

```
1.在Github上新建一个仓库，勾上Initialize this repository with a README(关于怎么删除仓库，找到导航栏最后一个Settings点进去看到本页末尾最后红色Danger Zone最后有个delete点击按提示操作即可)
2.在gitbook首页左侧点击Github一栏，右边title给书籍起个英文名,然后底下选对应的刚刚Github上创建的仓库
```



### 最后你就能在线看你的书籍了

参考链接：https://tonydeng.github.io/gitbook-zh/gitbook-howtouse/