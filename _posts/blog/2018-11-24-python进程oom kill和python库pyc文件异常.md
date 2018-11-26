---
layout: post
title: python进程OOM和python库pyc损坏的关系
categories: blog
description: consul进程使用cgroup进行资源限制，在内存不够的时候被oom kill了。再次重启的时候由于python库中pyc文件不能用，导致再次启动失败...oom和pyc文件损坏有关系吗？
---

背景：    
运行环境：centos的docker容器     
进程：consul，使用python脚本对consul进程尽心启动停止监控    
资源限制：cgroup进行资源限制，100M内存    
问题：consul进程内存超过100M，被系统oom kill掉了；   
python脚本再次启动失败，具体看的时候发现python脚本无法import原生的pyc文件；    
查看pyc文件发现大小较小，怀疑pyc文件损坏      


11-23 10:00 容器构造oom killer，一千三百多此，pyc文件不可用又出现一次。   


11-24 10:00 昨天在虚拟机里面浮现进程内存已经设置的很小了，但是没有oom killer。    
原因是虚拟机里面有swap，关闭swap后也会oom    


求助某python大神一起分析，名翔神

11-24 15:00 经翔神分析pyc，发现：
```
pyc文件不是新生成的，里面的时间还是2018年1月1号，看来不知道为什么oom之后它就被truncate了 

现在我理解应该是重启的时候不知道为什么触发了重新编译，然后可能在这种环境下触发了Python的import bug，Python好像的确有一个bug会导致写了一个空的codeobject进去

内容是完整的，只是内容是错误的

我们的业务进程OOM死掉后，业务进程重启，Python会启动，Python启动过程中会import标准库，比如encodings,re，这个过程涉及到读文件，但因为内存不过，首先读pyc失败，认为没有pyc，于是重新编译，重新编译的时候读文件又失败，触发了Python2中的一个bug，写入了一个空的pyc

今年刚修复,修复之后这种情况下启动的时候直接报IOError    

https://github.com/python/cpython/commit/f64c813de84011a84ca21d75a294861a9cc2dfdc    

https://bugs.python.org/issue25083 可以参考这个问题单，人家出现的问题现象基本和我们一致，一个大小极小的pyc文件，然后报不想关的错误，它里面也提供了重现的方法。    
           
```


11-26 22:00 周日跑了一天，虚拟机跑了20000+次oom，没有出现pyc文件损坏；经过翔神指点编译的带
补丁的python环境，也没有出现pyc问题   

11-26 23:00 三个容器，其中两个容器回退python验证，另外一个容器继续使用带补丁的python环境；持续观察   
     
