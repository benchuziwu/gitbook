### 1.git介绍

​         Git是一种非常流行的分布式版本控制系统，它和其他版本控制系统的主要差别在于Git只关心文件数据的整体是否发生变化，而大多数版本其他系统只关心文件内容的具体差异，这类系统（CVS，Subversion，Perforce，Bazaar 等等）每次记录有哪些文件作了更新，以及都更新了哪些行的什么内容。Git另一个比较好的地方在于绝大多数操作都可以在本地执行，而每个本地都可以从服务器获取一份完整的仓库代码，而且在没网的时候仍然可以修改和使用大部分命令，在方便的时候再跟服务器进行同步，这样可以更好的实现多人联合编程。

### 2.git的安装

###### (1)在Mac OS X上安装Git

如果你正在使用Mac做开发，有两种安装Git的方法。

一是安装homebrew，然后通过homebrew安装Git，具体方法请参考homebrew的文档：[http://brew.sh/](http://brew.sh/)。

第二种方法更简单，也是推荐的方法，就是直接从AppStore安装Xcode，Xcode集成了Git，不过默认没有安装，你需要运行Xcode，选择菜单“Xcode”->“Preferences”，在弹出窗口中找到“Downloads”，选择“Command Line Tools”，点“Install”就可以完成安装了。

###### (2)window安装git

msysgit是Windows版的Git，从[https://git-for-windows.github.io](https://git-for-windows.github.io/)下载（网速慢的同学请移步[国内镜像](https://pan.baidu.com/s/1kU5OCOB#list/path=%2Fpub%2Fgit)），然后按默认选项安装即可。

安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！

安装完成后，还需要最后一步设置，在命令行输入：

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

### 3.创建版本库

什么是版本库呢？版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

```
mkdir testGit
cd testGit
git init 
Initialized empty Git repository in /Users/michael/learngit/.git/

```

瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

```
.git目录默认是隐藏的，用ls -a可以显示隐藏文件
```

```
git help/git --help/ man git 来查看git的一些最常用的命令
```

### 4.git提交代码步骤

```
git status
git stash
git pull
git stash apply
git add .
git commit -m "u"
git push origin master
```

```
git clone 文件git地址（clone远程仓库）
```

 ### 5.git忽略规则及.gitignore规则不生效的解决办法

```
在文件根目录下执行touch .gitignore
编写.gitignore文件（可采用vim命令行编写）
将要忽略的文件目录写到该文件中
eg：
node_modules/
src/main/webapp/bower_components/
```

一般文件受git版本控制之后，如果要修改.gitignore添加要忽略的文件，（想把某些目录或文件加入忽略规则）会发现gitignore未生效，原因是.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交

```
git rm -rf --cached .
git add .
git commit -m "update .gitignore"
git push origin master
```

```
rm -rf .git
git init
git remote add origin 地址
git push -f -u origin master
```

```
git reset--mixed（git提交错误，stash时提示merge。fatal: git-write-tree: error building treesCannot save the current index state）http://stackoverflow.com/questions/5483213/fatal-git-write-tree-error-building-trees
```

参考链接：http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001373962845513aefd77a99f4145f0a2c7a7ca057e7570000

[http://www.cnblogs.com/wintersun/p/3930900.html](https://www.gitbook.com/book/dandelion1000/front-end-road/edit#)

