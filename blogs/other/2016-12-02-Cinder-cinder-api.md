---

title: 【OpenStack源码单排 一】cinder创建磁盘总述（一）
categories: 云计算
tags: cinder
---
*声明：本博客欢迎转发，但请保留原作者信息! 内容系本人学习、研究和总结，如有雷同，实属荣幸！*
 
*新浪微博：[@支支zHi小冬](http://weibo.com/u/1596536485/home?wvr=5)*

*博客地址：[http://xiaodongzhi.github.io/](http://xiaodongzhi.github.io/)*

*联系邮箱：517341003@qq.com*

 
openstack版本信息：ocata

# Cinder架构简述 #
![](http://i.imgur.com/sx59Ma3.jpg)
- Cinder API：提供cinder的REST API，通常部署在控制节点。
- Cinder Scheduler：负责Cinder请求调度，与nova-scheduler类似，根据请求中的调度条件，如volume-type，size等选择合适的后端存储来进行块存储管理。
- Cinder-volume：通过cinder-driver与各个厂商的存储设备进行交互，来进行块存储操作。


# Cinder创建磁盘流程图 #

## Cinder API ##
![](http://i.imgur.com/dmjYgMK.jpg)
重点讲下api中的几个task：

ExtractVolumeRequestTask：

1、参数校验，包含镜像创建卷时，卷属性与磁盘属性是否冲突，如磁盘容量小于镜像大小。
2、获取availability_zone信息，创建时指定>snapshot>source_volume>default_availability_zone

QuotaReserveTask：

预留Quota信息
检查cinder的配额，是否有足够配额用以创建磁盘。如果够，会更新quota信息减一。

EntryCreateTask：

数据库中创建volume信息。

QuotaCommitTask：

更新Quota信息到数据库

VolumeCastTask：

1、创建磁盘信息任务。从请求信息中获取host（就是创建卷的后端存储信息）。snapshot>source_volid>source_replicaid
2、如果从请求中获取到host信息，则直接调用cinder-volume的rpc接口在该host上创建卷
3、如果请求中未获取到host信息，则调用cinder-scheduler的rpc接口创建volume。

## Cinder Scheduler ##
![](http://i.imgur.com/JOMsp7i.jpg)

主要简单描述下scheduler中的几个Task：

ExtractSchedulerSpecTask：

获取用于调度的信息：

```python
{
    'volume_id': volume.id,
    'snapshot_id': snapshot_id,
    'image_id': image_id,
    'volume_properties': {
        'size': utils.as_int(volume.size, quiet=False),
        'availability_zone': volume.availability_zone,
        'volume_type_id': volume_type_id,
    },
    'volume_type': list(dict(vol_type).items()),
}
```

ScheduleCreateVolumeTask：

1、根据调度信息信息调度host（后端存储）
  1.1、查询符合条件的host
  1.2、weigh host
  1.3、选取最合适的主机
2、在选择的host上创建磁盘

## Cinder Volume ##
![](http://i.imgur.com/McVNxLJ.png)

cinder volume的处理机制与cinder api 和schedule一致，均通过任务队列完成。类似于工作流。

linear_flow相关介绍，参考[这里](http://stacker.top/blog/2016/12/06/python-taskflow/)

顺序执行：

1、ExtractVolumeRefTask

2、OnFailureRescheduleTask

3、ExtractVolumeSpecTask

4、NotifyVolumeActionTask

创建卷的主业务任务，详解见[下一章](http://stacker.top/blog/2016/12/02/Cinder%E4%BB%A3%E7%A0%81%E8%B5%B0%E8%AF%BB%E7%B3%BB%E5%88%97%E4%B8%80-%E5%88%9B%E5%BB%BA%E7%A3%81%E7%9B%98-%E4%BA%8C-cinder-volume/)

5、CreateVolumeFromSpecTask

6、CreateVolumeOnFinishTask

