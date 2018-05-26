---
layout: post
title: python with语句
categories: Python
---
*声明：本博客欢迎转发，但请保留原作者信息! 内容系本人学习、研究和总结，如有雷同，实属荣幸！*
 
*新浪微博：[@支支zHi小冬](http://weibo.com/u/1596536485/home?wvr=5)*

*博客地址：[http://xiaodongzhi.github.io/](http://xiaodongzhi.github.io/)*

*联系邮箱：517341003@qq.com*

 

一、with语句解决什么问题?

常规写法：

    file = open("/tmp/foo.txt")
    try:
        data = file.read()
    finally:
        file.close()

python with语句：

    with open("/tmp/foo.txt")
     as file:
        data = file.read()

with语句可以简洁的代替try...cath...finally写法

二、with如何工作
    #!/usr/bin/env
     python
    #
     with_example01.py
     
     
    class Sample:
        def __enter__(self):
            print "In __enter__()"
            return "Foo"
     
        def __exit__(self, type, value, trace):
            print "In __exit__()"
     
     
    def get_sample():
        return Sample()
     
     
    with
     get_sample() as sample:
    print "sample:",
     sample

紧跟with后面的语句被求值后，返回对象的__enter__()方法被调用。这个方法的返回值将被赋值给as后面的变量。当with后面的代码块全部被执行完之后，将调用前面返回对象的__exit__()方法。

    In __enter__()
    sample:
     Foo
    In __exit__()