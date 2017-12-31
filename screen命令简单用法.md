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

对我来说，以上就足够了，有特定需求时再说。

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
