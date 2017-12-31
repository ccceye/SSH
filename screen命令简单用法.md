# screen 命令简单用法

screen是一款由GNU计划开发的用于命令行终端切换的自由软件，实现全屏窗口管理的功能，能够混合多个工作到一个终端上。一般linux系统中自带有screen命令.

现在很多时候我们的开发环境都已经部署到云端了，直接通过SSH来登录到云端服务器进行开发测试以及运行各种命令，一旦网络中断，通过SSH运行的命令也会退出，这个发让人发疯的。

好在有screen命令，它可以解决这些问题。我使用screen命令已经有三年多的时间了，感觉还不错。

## 新建一个Screen Session

```
$ screen -S screen_session_name
```

通过SSH登远程服务器后，直接在命令行下运行以下命令新建一个screen会话：

`screen`

也可以指定会话的名称，以下即创建以noalgo为名称的会话：

`screen -S loli`

也可以在新建会话时指定要运行的程序，以下运行了vi编辑器，注意此时退出vi编辑器即表示退出了screen会话。

`screen -S loli vi helloworld.c`

新建会话后即进入了screen的世界，在这里做的事情和在普通的shell中的事情没有什么区别，只是此时的会话是可以进行恢复的，即使发生网络中断，也可以通过再次运行screen命令回到刚才的会话中，而且，再次回来时屏幕上显示的是刚才的画面，而如果程序动态运行时，此时显示最新的结果。

**如果有事需要离开，而服务器上的程序需要同时在运行，此时可以通过命令d分离会话。在screen会话中进行的操作都是以ctrl+a开始，所以分离时需要先按下ctrl+a，然后再按d.**

此时会回到原先的SSH窗口，就可以随意关掉SSH去干其他事情了。

当要回去的时候可以先通过SSH进行登录，然后运行以下命令查看系统中已有的screen会话：`screen -list(ls)` 得到的结果类似为:
```
[Loli@LoliconServer ~]$ screen -list
There is a screen on:
	  8530.loli	(Detached)
 1 Socket in /var/run/screen/S-Loli.
 ```

然后可以通过`screen -r 8530`回到会话中，也可以输入名字：`screen -r loli`

## 将当前Screen Session放到后台

```
$ CTRL + A + D
```

## 唤起一个Screen Session

```
$ screen -r screen_session_name
```

## 分享一个Screen Session

```
$ screen -x screen_session_name
```

通常你想和别人分享你在终端里的操作时可以用此命令。

通过`screen -x`命令可以实现会话共享，此时多个用户登录到同一个会话中，如果他们同时处于同一个窗口下时，彼此的操作会同步给每一个用户，即达到共享桌面的效果。
```
# 创建一个名称为“BENET”的共享屏幕会话
screen –S BENET
# 连接到共享屏幕，在另一个终端上
screen -x BENET
```

## 终止一个Screen Session

```
$ exit
$ CTRL + D
```

## 查看一个screen里的输出

当你进入一个screen时你只能看到一屏内容，如果想看之前的内容可以如下：

```
$ Ctrl + a ESC
```

以上意思是进入Copy mode，拷贝模式，然后你就可以像操作VIM一样查看screen session里的内容了。

可以 Page Up 也可以 Page Down。

## screen进阶

### 多窗口

在普通的shell环境中，如果要同时执行多个程序，可以通过ctrl+z，以及fg和bg等命令交替执行，但screen提供了多窗口的功能同样可以达到这个目的。
通过screen命令进入了screen会话默认的一个窗口，通过Ctrl + a + c命令可以新建一个窗口并进入新的窗口，在不同的窗口间切换可以通过下面两个命令进行，分别是进入下一个和前一个窗口：
```
Ctrl + a + n
Ctrl + a + p
```
使用以下命令可以查看当前共有几个窗口，标注*号的为当前所在的窗口：

`Ctrl + a + w`

使用以下命令强行关闭一个窗口，如果当前只剩下最后一个窗口，则终止当前的会话：

`Ctrl + a + k`

使用exit命令也可以达到同样的效果，当使用多个窗口时，可以通过将屏幕分割成几个区域来提高效率。使用以下命令进行分屏，分别是水平分割和垂直分割：
```
Ctrl + a + S
Ctrl + a + |
```
拥有多个屏幕时，使用以下命令进行切换：

`Ctrl + a + Tab`

使用以下命令关闭某个分屏，

`Ctrl + a + X`

或者关闭处当前区域的所有其他区域：

`Ctrl + a + Q`


### Screen详细参数

以上是通过简单的例子介绍screen的常见用法，下面对其参数进行详细介绍。screen的命令语法为：

`screen [-AmRvx -ls -wipe][-d ][-h <line>][-r ][-s ][-S ]`

**其中的参数意义如下：**
```
-A：将所有的视窗都调整为目前终端机的大小。
-d：分离指定的screen会话。
-h：指定视窗的缓冲区行数。
-m：即使目前已在会话中的screen会话，仍强制建立新的screen会话。
-r：恢复分离的screen会话。
-R：先试图恢复离线的会话。若找不到离线的会话，即建立新的screen会话。
-s：指定建立新视窗时，所要执行的shell。
-S：指定screen会话的名称。
-v：显示版本信息。
-x：恢复之前离线的screen会话。
-ls：显示目前所有的screen会话。
-list：显示目前所有的screen会话。
-wipe：检查目前所有的screen会话，并删除已经无法使用的screen会话。
```
**在每个screen会话中，可以使用的命令如下。注意，screen的命令都是以ctrl+a(C-a)开始的，以下省略C-a而直接以后面的按键替代：**
```
?：Help，显示按键绑定情况。
c：Create，创建新的窗口。
n：Next，切换到下个窗口。
p：Previous，切换到前一个窗口。
M：查看活动状态。
x：锁住当前的窗口，需用用户密码解锁。
d：Detach，暂时离开当前会话，此后可以恢复。
z：把当前会话放到后台执行，可以使用shell的fg命令回去。
w：Windows，列出已创建的窗口。
t：Time，显示当前时间。
K：Kill，强行关闭当前的窗口。
[0..9]：切换到第 0..9个窗口。
[Space]：由窗口0顺序切换到窗口9。
C-a：在两个最近使用的窗口间切换。
S：水平分屏。
|：垂直分屏。
X：关闭当前分屏。
Q：关闭除当前分屏的所有分屏。
[Tab]：在分屏中切换。
[：Copy,进入拷贝模式，此时可以回滚、搜索、复制，就像用使用vi一样。
]：Paste，粘贴刚刚在拷贝模式选定的内容。
```
其中在拷贝模式下可以使用的命令包括
```
C-b：Backward，PageUp。
C-f：Forward，PageDown。
H：High，将光标移至左上角。
L：Low，将光标移至左下角。
0：移到行首。
$：移到行末。
w：forward one word，前移一个字。
b：backward one word，后移一个字。
Space：第一次按标记起点，第二次按标记终点。
Esc：结束copy mode。
```
这里列的也不是全部的参数，需要更详细的内容，可以直接通过以下命令进行获取：man screen


## screenrc

[.screenrc](.screenrc)

如果没有就在~下新建该文件

然后添加一行配置

```
escape ^Bt
```

表示其他(例如ctrl+B)替换掉默认的ctrl+A, 

因为ctrl+A往往是快速回到命令的头部的快捷键，非常常用和方便

## End

screen命令很好用，但是最让人头痛的是`CTRL+A`命令和BASH里的快捷键重复了，我不觉得替换一下快捷键是个很好的解决方案，所以这个问题一直存在我这里。

这里有更详细的说明：<http://www.ibm.com/developerworks/cn/linux/l-cn-screen/>
