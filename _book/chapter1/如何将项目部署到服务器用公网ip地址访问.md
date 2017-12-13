#### 如何将项目部署到服务器用公网ip地址访问

```
1.创建阿里云账号，购买服务器，返回实例列表得到公网ip和内网ip地址，
```

```
2.打开本地控制台输入ssh root@182.92.102.97(公网ip地址)
vim /etc/ssh/sshd_config//修改ssh的配置文件，可以修改端口等等信息，对服务器简单加固
/etc/init.d/sshd restart（reload）//重新加载配置文件
 ssh -p 9393 root@182.92.102.97//改变端口(9393)连接远程服务器，登录到该服务器
```

```
3.接下来可以在该服务器上安装git，node，npm，anywhere,bower
yum install -y git
接下来克隆远程git项目时，出现一个问题：error: The requested URL returned error: 401 Unauthorized while accessing 
原因是Centos 6.5默认安装的是git 1.7.X 版本，使用过程中会有一些奇怪的问题，对于用户名、密码支持不是很友好。
```

```
4.将Centos6.5上的git更新到2.0.5，方法如下
(1).安装编译git时需要的包
# yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
# yum install  gcc perl-ExtUtils-MakeMaker
(2).删除已有的git
# yum remove git
(3).下载git源码
# cd /usr/src
# wget https://www.kernel.org/pub/software/scm/git/git-2.0.5.tar.gz
# tar xzf git-2.0.5.tar.gz//解压
(4).编译安装
# cd git-2.0.5
# make prefix=/usr/local/git all
# make prefix=/usr/local/git install
# echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc
# source /etc/bashrc
(5).检查一下版本号
# git --version
git version 2.0.5
接下来执行git clone就可以了
```

```
5.下载好远程项目之后，进入该目录，执行anywhere 8888(端口) 命令，启动服务，打开公网地址182.92.102.97：8888就可以访问该页面了
```

```
anywhere命令补充说明
  anywhere --help // print help information
  anywhere // 8000 as default port, current folder as root
  anywhere 8888 // 8888 as port
  anywhere -p 8989 // 8989 as port
```

```
如何可以不执行anywhere，也可以一直开启服务通过公网ip访问该网站
anywhere 8888 & //进入后台
jobs即可
```

```
补充：rm -rf filename删除文件
通过nvm安装管理nodejs
```

```
如何将本地文件上传至服务器
scp -P9393 node-v6.9.1.tar.gz root@182.92.102.97:/tmp/
将本地文件上传至远程服务器，scp -P端口 加上文件名 远程ip：文件目录

```

```
如何忽略打包不需要上传的文件
```



```
可以同时删除多个文件，也可以删除除了此文件以外的所有文件
ls |grep -v "aaa" |grep -v "yyy" |xargs -i rm -rf {}

ls |grep -v "publish.zip" |xargs -i rm -rf {}
```





### 参考链接

http://jingyan.baidu.com/article/0eb457e529fbb203f1a905bd.html