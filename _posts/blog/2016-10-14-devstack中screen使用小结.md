---
layout: post
title: devstack中screen使用小结
categories: blog
---


## 一、`screen -r stack`不成功
![](http://i.imgur.com/iNRBxiv.jpg)
screen -list：查看当前用户开启的所有screen
此时看到已经有个screen开着，stack正在连接使用，所以此时再用screen -r stack连接，就会
    `There is no screen to be resumed matching stack`

此时需要先将已经连接的screen detach掉。执行：`screen -d stack`。即可切断原有连接。
再次执行`screen -r stack`，就能连接上了。报错：

##二、在screen视图下切换窗口 
移到下一个window：`CRTL + A + n`

移到前一个window：`CRTL + A + p`

## 三、screen Cannot open your terminal '/dev/pts/1'问题 


如下内容转自：http://blog.sina.com.cn/s/blog_704836f401010osn.html

 问题描述:

 

  ` userA`首先登录系统，使用`screen`开启了一个`session`，然后`detach`这个窗口。

   `userB`然后登录系统，通过`su - userA` 变成`userA`，然后使用`screen -r` 恢复之前`detached`窗口，这时系统报如下错误:

   `*Cannot open your terminal '/dev/pts/1' - please check.*`

 

解决方法:

 

   `userB`在` su - userA`以后，执行如下命令即可:

 

  ` script /dev/null`

 

注意: 有人提到 `chmod 777 /dev/pts/1`，这么干的人真是误人子弟，虽然这么做的确能解决这个问题，但是会带来极大的安全问题！！！

 

为什么这条命令能解决问题?

 

一般人看到上面这里估计就马上回去试验了，但是，等等，你不想知道为什么这个命令会有作用吗？它是怎么起作用的呢？

 

我们来过一遍整个的操作步骤:

 

首先，`usera`登录到系统中，我们使用tty命令查看一下分配给他的`tty`，然后看一下这个`tty`的权限，然后用户执行`screen`命令。

 

    usera@localhost ~ $ ssh usera@remotehost
    usera@remotehost ~ $ tty
    /dev/pts/1
    usera@remotehost ~ $ ls -l /dev/pts/1
    crw--w---- 1 usera tty 136, 1 2011-01-09 20:14 /dev/pts/1
    usera@remotehost ~ $ screen
    
 

我们观察上边的输出，发现`usera`对于`/dev/pts/1`具有读写权限，它所在组成员对这个tty具有写权限，其他用户不能访问这个`tty`。

 

然后，userb也登录到系统中，同样我们使用tty命令查看一下分配给他的tty，然后看一下这个`tty`的权限

    userb@localhost ~ $ ssh userb@remotehost
    userb@remotehost ~ $ tty
    /dev/pts/2
    userb@remotehost ~ $ ls -l /dev/pts/2
    crw--w---- 1 userb tty 136, 2 2011-01-09 20:20 /dev/pts/2

 

观察输出，userb被分配了`/dev/pts/2`，也是对于`/dev/pts/2`具有读写权限，它所在组成员对这个`tty`具有写权限，其他用户不能访问这个`tty`。

 

然后`userb`通过`su - usera`命令变成`usera`，同样我们使用`tty`命令查看一下分配给他的`tty`，然后看一下这个`tty`的权限

    userb@remotehost ~ $ sudo su - usera
    [sudo] password for userb:
    usera@remotehost ~ $ tty
    /dev/pts/2
    usera@remotehost ~ $ ls -l /dev/pts/2
    crw--w---- 1 userb tty 136, 2 2011-01-09 20:20 /dev/pts/2

 

AHA!!  注意了，我们看到虽然`userb`已经变成了`usera`，但是他所使用的`tty`并没有改变，仍然是`/dev/pts/2`。这就是为什么执行`screen`命令会报错的原因了，因为所有命令此时是使用`usera`帐户执行的，但是`/dev/pts/2`的读写权限属于`userb`，所以所有试图控制`/dev/pts/2`的访问都被拒绝了！

 

那么我们接下来看一下 `script /dev/null`做了些什么，使得`screen`命令能执行呢？


    usera@remotehost ~ $ script /dev/null
    Script started, file is /dev/null
    usera@remotehost ~ $ tty
    /dev/pts/3
    usera@remotehost ~ $ ls -l /dev/pts/3
    crw--w---- 1 usera tty 136, 3 2011-01-09 20:36 /dev/pts/3


AHA!!! 看到了吗？我们实际上是得到了一个新的`tty ---> /dev/pts/3`，因此`screen`命令能够执行了，因为` /dev/pts/3`这个`tty`的所有者是`usera`！
