# screen 命令简单用法

现在很多时候我们的开发环境都已经部署到云端了，直接通过SSH来登录到云端服务器进行开发测试以及运行各种命令，一旦网络中断，通过SSH运行的命令也会退出，这个发让人发疯的。

好在有screen命令，它可以解决这些问题。我使用screen命令已经有三年多的时间了，感觉还不错。

## 新建一个Screen Session

```
$ screen -S screen_session_name
```

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
