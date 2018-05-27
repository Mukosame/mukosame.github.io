---
layout: post
title: taskflow
categories: blog
---

一、Flow类

```python
class taskflow.flow.Flow(name, retry=None)
Bases: object
```
flow定义了多个task之间的关系，可以在flow中添加多个task，也可以将其他flow作为子flow添加到flow中。flow可以定义这些task或者flow之间相互依存的关系。
这些相互依存的关系被flow的子类体现。


name：flow的名称

retry：当flow执行时并且有错误产生时，retry controller将会产生作用。


二、Linear flow

```python
class taskflow.patterns.linear_flow.Flow(name, retry=None)[source]
Bases: taskflow.flow.Flow
```

线性流：
【描述】线性流作为一个整体执行或者回滚。也就是说线性流中task和flow被顺序执行或者逆序回滚。
【限制】前一个任务的输出，必须是后一个任务的输入

add(*items)
添加task或者flow到线性流


参考：https://wiki.openstack.org/wiki/TaskFlow