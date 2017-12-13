**一、more命令**

more功能类似 cat ，cat命令是整个文件的内容从上到下显示在屏幕上。 more会以一页一页的显示方便使用者逐页阅读，而最基本的指令就是按空白键（space）就往下一页显示，按 b 键就会往回（back）一页显示，而且还有搜寻字串的功能 。more命令从前向后读取文件，因此在启动时就加载整个文件。

**1．命令格式：**

more [-dlfpcsu ] [-num ] [+/ pattern] [+ linenum] [file ... ] 

**2．命令功能：**

more命令和cat的功能一样都是查看文件里的内容，但有所不同的是more可以按页来查看文件的内容，还支持直接跳转行等功能。

**3．命令参数：**

+n      从笫n行开始显示

-n       定义屏幕大小为n行

+/pattern 在每个档案显示前搜寻该字串（pattern），然后从该字串前两行之后开始显示  

-c       从顶部清屏，然后显示

-d       提示“Press space to continue，’q’ to quit（按空格键继续，按q键退出）”，禁用响铃功能

-l        忽略Ctrl+l（换页）字符

-p       通过清除窗口而不是滚屏来对文件进行换页，与-c选项相似

-s       把连续的多个空行显示为一行

-u       把文件内容中的下画线去掉

**4．常用操作命令：**

Enter    向下n行，需要定义。默认为1行

Ctrl+F   向下滚动一屏

空格键  向下滚动一屏

Ctrl+B  返回上一屏

=       输出当前行的行号

：f     输出文件名和当前行的行号

V      调用vi编辑器

!命令   调用Shell，并执行命令 

q       退出more

**5．命令实例：**

实例1：显示文件中从第3行起的内容

命令：

```
aijian.shi@U-aijian-shi:~/ALM$ cat test.log                   #显示所有日志内容
aijian.shi@U-aijian-shi:~/ALM$ more +3 test.log               #从第三行开始显示日志内容
```

输出：

```
 1 aijian.shi@U-aijian-shi:~/ALM$ cat test.log
 2 2016-8-1 aijian.shi
 3 2016-8-2 yafang.wei
 4 2016-8-3 hong.zhan
 5 2016-8-4 yuyan.zhang
 6 2016-8-5 senlin.zhao
 7 2016-8-6 yanbin.liu
 8 2016-8-7 hui.liu
 9 2016-8-8 yanhua.liu
10 2016-8-9 baoixn.cui
11 2016-8-10 ge.song
12 2016-8-11 zhongjun.zhen
13 2016-8-12 qiu.liao
aijian.shi@U-aijian-shi:~/ALM$ more +3 test.log
2016-8-3 hong.zhan
2016-8-4 yuyan.zhang
2016-8-5 senlin.zhao
2016-8-6 yanbin.liu
2016-8-7 hui.liu
2016-8-8 yanhua.liu
2016-8-9 baoixn.cui
2016-8-10 ge.song
2016-8-11 zhongjun.zhen
2016-8-12 qiu.lia
```

实例2.将日志内容设置为每屏显示4行

命令：

```
aijian.shi@U-aijian-shi:~/ALM$ more -4 test.log
```

输出：

```
 1 aijian.shi@U-aijian-shi:~/ALM$ more -4 test.log
 2 2016-8-1 aijian.shi
 3 2016-8-2 yafang.wei
 4 2016-8-3 hong.zhan
 5 2016-8-4 yuyan.zhang
 6 
 7 ...skipping one line              #这里使用ctrl+F或者空格键来滚屏
 8 2016-8-6 yanbin.liu
 9 2016-8-7 hui.liu
10 2016-8-8 yanhua.liu
11 2016-8-9 baoixn.cui
12 
13 ...skipping one line
14 2016-8-11 zhongjun.zhen
15 2016-8-12 qiu.liao
```

实例3.从文件中查找第一个出现"liu"字符串的行，并从该处前两行开始显示输出

命令：

```
aijian.shi@U-aijian-shi:~/ALM$ more +/liu test.log
```

 

输出：

```
 1 aijian.shi@U-aijian-shi:~/ALM$ more +/liu test.log
 2 
 3 ...skipping
 4 2016-8-4 yuyan.zhang
 5 2016-8-5 senlin.zhao
 6 2016-8-6 yanbin.liu
 7 2016-8-7 hui.liu
 8 2016-8-8 yanhua.liu
 9 2016-8-9 baoixn.cui
10 2016-8-10 ge.song
11 2016-8-11 zhongjun.zhen
12 2016-8-12 qiu.liao
```

实例4.当一个目录下的文件内容太多，可以用more来分页显示。这得和管道 | 结合起来

命令：

```
aijian.shi@U-aijian-shi:~/ALM$ cat test.log | more -5   #“|”表示管道，作用是可以将前面命令的输出当做后面命令的输入

```

输出：

```
 1 aijian.shi@U-aijian-shi:~/ALM$ cat test.log | more -5
 2 2016-8-1 aijian.shi
 3 2016-8-2 yafang.wei
 4 2016-8-3 hong.zhan
 5 2016-8-4 yuyan.zhang
 6 2016-8-5 senlin.zhao
 7 
 8 ...skipping one line
 9 2016-8-7 hui.liu
10 2016-8-8 yanhua.liu
11 2016-8-9 baoixn.cui
12 2016-8-10 ge.song
13 2016-8-11 zhongjun.zhen
14 --more--  
```

**二、less指令**

less 工具也是对文件或其它输出进行分页显示的工具，应该说是linux正统查看文件内容的工具，功能极其强大。less 的用法比起 more 更加的有弹性。 在 more 的时候，我们并没有办法向前面翻， 只能往后面看，但若使用了 less 时，就可以使用 [pageup] [pagedown] 等按 键的功能来往前往后翻看文件，更容易用来查看一个文件的内容！除此之外，在 less 里头可以拥有更多的搜索功能，不止可以向下搜，也可以向上搜。

**1．命令格式：**

less [参数]  文件 

**2．命令功能：**

less 与 more 类似，但使用 less 可以随意浏览文件，而 more 仅能向前移动，却不能向后移动，而且 less 在查看之前不会加载整个文件。

**3．命令参数：**

-b <缓冲区大小> 设置缓冲区的大小

-e  当文件显示结束后，自动离开

-f  强迫打开特殊文件，例如外围设备代号、目录和二进制文件

-g  只标志最后搜索的关键词

-i  忽略搜索时的大小写

-m  显示类似more命令的百分比

-N  显示每行的行号

-o <文件名> 将less 输出的内容在指定文件中保存起来

-Q  不使用警告音

-s  显示连续空行为一行

-S  行过长时间将超出部分舍弃

-x <数字> 将“tab”键显示为规定的数字空格

/字符串：向下搜索“字符串”的功能

?字符串：向上搜索“字符串”的功能

n：重复前一个搜索（与 / 或 ? 有关）

N：反向重复前一个搜索（与 / 或 ? 有关）

b  向后翻一页

d  向后翻半页

h  显示帮助界面

Q  退出less 命令

u  向前滚动半页

y  向前滚动一行

空格键 滚动一行

回车键 滚动一页

[pagedown]： 向下翻动一页

[pageup]：   向上翻动一页

**4．使用实例：**

实例1：ps查看进程信息并通过less分页显示同时显示行号

命令：

```
aijian.shi@U-aijian-shi:~/ALM$ ps -ef|less -N
```

输出

```
 1       1 UID        PID  PPID  C STIME TTY          TIME CMD
 2       2 root         1     0  0 Aug08 ?        00:00:00 /sbin/init
 3       3 root         2     0  0 Aug08 ?        00:00:00 [kthreadd]
 4       4 root         3     2  0 Aug08 ?        00:00:02 [ksoftirqd/0]
 5       5 root         6     2  0 Aug08 ?        00:00:00 [migration/0]
 6       6 root         7     2  0 Aug08 ?        00:00:00 [watchdog/0]
 7       7 root         8     2  0 Aug08 ?        00:00:00 [migration/1]
 8       8 root         9     2  0 Aug08 ?        00:00:00 [kworker/1:0]
 9       9 root        10     2  0 Aug08 ?        00:00:01 [ksoftirqd/1]
10      10 root        11     2  0 Aug08 ?        00:00:00 [watchdog/1]
11      11 root        12     2  0 Aug08 ?        00:00:00 [migration/2]
12      12 root        14     2  0 Aug08 ?        00:00:01 [ksoftirqd/2]
13      13 root        15     2  0 Aug08 ?        00:00:00 [watchdog/2]
14      14 root        16     2  0 Aug08 ?        00:00:00 [migration/3]
15      15 root        18     2  0 Aug08 ?        00:00:01 [ksoftirqd/3]
16      16 root        19     2  0 Aug08 ?        00:00:00 [watchdog/3]
17      17 root        20     2  0 Aug08 ?        00:00:00 [migration/4]
18      18 root        22     2  0 Aug08 ?        00:00:00 [ksoftirqd/4]
19      19 root        23     2  0 Aug08 ?        00:00:00 [watchdog/4]
20      20 root        24     2  0 Aug08 ?        00:00:00 [migration/5]
21      21 root        26     2  0 Aug08 ?        00:00:00 [ksoftirqd/5]
22      22 root        27     2  0 Aug08 ?        00:00:00 [watchdog/5]
23      23 root        28     2  0 Aug08 ?        00:00:00 [migration/6]
24 :
```

实例2.浏览多个文件

命令：

```
aijian.shi@U-aijian-shi:~/ALM$ less test2.log test.log
```

输出：

```
 1     1  ifconfig
 2     2  ping www.baidu.com
 3     3  ifconfig
 4     4  //10.128.161.108/share
 5     5  10.128.161.108/share
 6     6  ssh
 7     7  keygen
 8     8  trsa
 9     9  ssh
10    10  .ssh/
11 
12 
13  test2.log (file 1 of 2) (END) - Next: test.log
```

[![复制代码](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

说明：

输入 ：n后，切换到 test.log

输入 ：p 后，切换到test2.log

ps：当正在浏览一个文件时，也可以使用 :e命令 打开另一个文件。

命令：

```
less file1

:e file2
```

 

**5．附加备注**

1.全屏导航

ctrl + F - 向前移动一屏

ctrl + B - 向后移动一屏

ctrl + D - 向前移动半屏

ctrl + U - 向后移动半屏

 

2.单行导航

j - 向前移动一行

k - 向后移动一行

 

3.其它导航

G - 移动到最后一行

g - 移动到第一行

q / ZZ - 退出 less 命令

 

4.其它有用的命令

v - 使用配置的编辑器编辑当前文件

h - 显示 less 的帮助文档

&pattern - 仅显示匹配模式的行，而不是整个文件

 

5.标记导航

当使用 less 查看大文件时，可以在任何一个位置作标记，可以通过命令导航到标有特定标记的文本位置：

ma - 使用 a 标记文本的当前位置

'a - 导航到标记 a 处

 

6.查找

more, less 都具备查找功能，按/ 然后输入要找的字串，再按 Enter 即可，按 n(next) 会继续找，大写的 N 则是往回(上)找，按 q(quit)或者ZZ离开