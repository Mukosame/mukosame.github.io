---
title: OpenStack实践之devstack
categories: 云计算
---
按照devstack官网介绍，执行完`./devstack.sh`后，经过各种网络问题后，安装完成。


----------

1、环境变量的配置
安装完成后直接执行 `nova list`，会提示
`You must provide a username or user ID via  --os-username, env[OS_USERNAME] or  --os-user_id, env[OS_USER_ID]`

好，接着导入环境变量：
进入devstack目录下。执行`source openrc admin demo`

----------

2、创建虚拟机时，规格与镜像选择要匹配
创建个虚拟机试试：
![title](/images/openstack/nova-boot-error-vm.jpg).
随便创建了个虚拟机，发现虚拟机不能用，状态为ERROR。
使用`nova show {instanceId}`查看虚拟机message。发现“aborted: Flavor's disk is too small for requested image”

好吧，也就是说创建虚拟机时候，选择的flavor的规格一定要满足所选镜像所需要的规格大小。


之后发现，我的ubuntu虚拟机的内存2G，硬盘20G貌似不够创建一个虚拟机。
重新折腾了个3G内存，100G磁盘的虚拟机。

----------

3、关于hypervisor容量统计信息的疑问？
之后发现一个奇怪的现象。就是hypervisor页面上关于容量的统计与实际情况不一致，如下图：
![title](/images/openstack/hypervisor疑问.jpg).
----------

4、创建虚拟机成功后，horizon页面连接改虚拟机的VNC控制台失败，后台执行`nova get-vnc-console VM01 onvnc`。提示：

*
ClientException: The server has either erred or is incapable of performing the requested operation. (HTTP 500) (Request-ID: req-c2b3ab29-3d55-412b-aa6f-170835ca5ea4)
ERROR (ClientException): The server has either erred or is incapable of performing the requested operation. (HTTP 500) (Request-ID: req-c2b3ab29-3d55-412b-aa6f-170835ca5ea4)*

由于该信息是由nova-console提供的。所以先检查下nova-console服务是否正常。
执行`nova-manage service list`。结果如下图：
![title](/images/openstack/nova-manage service list.JPG).


观察到没有`nova-console`服务。
尝试使用`nova-console`命令启动`nova-console`服务。启动成功后再次执行`nova get-vnc-console VM01 onvnc`。则获取VM01虚拟机的VNC信息成功，复制到浏览器也可以打开VNC页面。horizon页面控制台也成功打开。

注：启动`nova-console`服务后，还必须启动`nova-consoleauth`服务，才可以正常使用vnc

----------

5、安装devstack的虚拟机如果重启了执行`rejoin-stack.sh`,即可完成openstack各个服务的重启。

----------

6、安装GNOME后，重启系统默认进入GNOME登陆界面，输入正确的用户名密码，不能进入桌面。
计划在server版本安装图形界面后使用eclipse+pydev调试openstack源码，依次安装eclipse，GNOME。
重启服务器后，默认进入GNOME登陆界面，输入正确的用户名密码，不能进入桌面。`crtl+alt+f1`切换到命令行界面。


