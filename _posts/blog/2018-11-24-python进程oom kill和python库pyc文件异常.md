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
     
```




