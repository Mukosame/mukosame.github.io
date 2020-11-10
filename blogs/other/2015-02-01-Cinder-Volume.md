---
title: volume实践
categories: 云计算
---

Openstack中的Volume数据Cinder中快存储的概念，用于虚拟机使用。
volume有很多中文称呼（不知道大家平时这样叫准确不）：卷，硬盘，云磁盘。




今天创建volume一直失败，日志一直提示：
![title](/images/openstack/cinder/no valid host.jpg).


反复检查了cinder的配置文件`cinder.conf`中，关于backend的配置：
![title](/images/openstack/cinder/cinder.conf.jpg).


然后再反复看下cinder schedule的日志。
发现:
![title](/images/openstack/cinder/LVM update log.jpg).


好吧，不知道这个从哪来的，为啥一直有这个日志打印。暂且先记着这个问题继续尝试下。
将名为`default`的`volume_type`的`extra_specs`中的`volume_backend_name`的值修改为`LVM`.


重新创建volume。创建成功。
![title](/images/openstack/cinder/succeed volume created.jpg). 


在创建成功的喜悦同时，也陷入深深的疑问中。明明配置的`volume_backend_name`的值为`default`。为啥后来莫名其妙的变成`LVM`了？

总结以上描述的事实：
1、创建volume的时候，选择的volume type指定的backend一定要是实实在在存在的，否则将无法找到合适的主机创建。

----------


重启了devstack虚拟机后，发现。。。。我去。
vgdisplay，pvscan都查询不到任何东西。
但是在`/opt/stack/data`目录下磁盘文件还是存在的。
查看了devstack的安装脚本stack.sh中关于vg相关代码。并且执行`sudo losetup -f --show stack-volumes-default-backing-file`后，vg，pv等均显示出来了。


至于losetup是干啥的？google之：

----------
再次重启后，vgdisplay等等命令又查不到了。郁闷。。。
决定手动创建vg。

由于我的openstack虚拟机磁盘本来就比较小，所以使用vmware workstations中虚拟机磁盘设置页面的“磁盘实用工具”的“扩展”磁盘容量功能。
注：扩展磁盘需要关闭虚拟机情况下使用。扩展完成后，打开虚拟机


一、手动分区。需要手动为新增的磁盘空间进行分区。分区命令如下：
1、`sudo fdisk /dev/sda`

2、`n` 添加primary分区

3、`t` 命令将新添加的分区类型修改为`8e`

4、配置成功后，输入`w`完成分区配置的保存。

5、`partprobe` 使分区生效，无需重启。


二、完成分区后，创建pv
1、`pvcreate /dev/sda4` 创建pv

2、创建成功后，可以使用`pvdisplay`查看刚才创建的`pv`


三、创建vg
1、`vgcreate cinder-volumes /dev/sda4`

2、创建成功后，可以使用`vgdisplay`查看刚才创建的`vg`

注：`vg`名称要使用`cinder-volumes`，要不cinder启动的时候会报找不到该`vg`


搞定之后，重启`cinder-schedule`，`cinder-volume`。
重新创建`volume`，创建成功。
使用`lvdisplay`，可以看到刚才创建的`volume`对应的`lv`。
`lv`的UUID等于cinder创建volume的ID

