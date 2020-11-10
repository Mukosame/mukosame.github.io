---

title: 【OpenStack实践系列 五 】nova 创建多盘虚拟机
categories: 云计算
tags: nova
---
*声明：本博客欢迎转发，但请保留原作者信息! 内容系本人学习、研究和总结，如有雷同，实属荣幸！*
 
*新浪微博：[@支支zHi小冬](http://weibo.com/u/1596536485/home?wvr=5)*

*博客地址：[http://xiaodongzhi.github.io/](http://xiaodongzhi.github.io/)*

*联系邮箱：517341003@qq.com*

 
openstack版本信息：ocata


----------


创建多盘虚拟机

本次实践，是以镜像（--iamge）创建虚拟机。故第一步查询image id。

1、查询image id

```python
root@jenkins:~# glance image-list
+--------------------------------------+---------------------+
| ID                                   | Name                |
+--------------------------------------+---------------------+
| 96ee0c5e-f277-482c-94b0-d8c91dd29d3a | cirros-0.3.4-x86_64 |
| f5308779-3ab3-460c-95c9-2791211e640b | VSA                 |
+--------------------------------------+---------------------+

```

2、查询network信息

```python
root@jenkins:~# neutron net-list
+--------------------------------------+---------+---------------------------------------------------------+
| id                                   | name    | subnets                                                 |
+--------------------------------------+---------+---------------------------------------------------------+
| a3c9e669-7788-4e7b-913f-57904f307289 | net101  | 6275d663-2d03-43b6-8376-49ad75a4ce8a 10.101.0.0/24      |
| cc8f8880-728c-49e3-9d68-7c91cd142ecd | private | 26bca241-c876-4d15-a363-00dfa2d7ff87 10.0.0.0/24        |
|                                      |         | 56c317bd-57be-4632-9eab-b4572f634365 2001:db8:8000::/64 |
| fe8cfa17-2cf8-4c6d-83c5-c523441386b3 | public  | 8a816c26-c4e0-4bf8-b9e5-d2dca0a6b965 172.24.4.0/24      |
|                                      |         | 5c5044a8-2e18-4887-8fb3-31137a9c6d40 2001:db8::/64      |
+--------------------------------------+---------+---------------------------------------------------------+

```

3、查询创建好的空卷信息

```python
root@jenkins:~# cinder list
+--------------------------------------+-----------+------------+------+-------------+----------+--------------------------------------+
| ID                                   | Status    | Name       | Size | Volume Type | Bootable | Attached to                          |
+--------------------------------------+-----------+------------+------+-------------+----------+--------------------------------------+
| 378fb335-91bc-4985-a1a1-6b2c3edb40f5 | available | zd_disk_01 | 10   | DS01        | false    |                                      |
| 90fc22bf-c45f-4db7-bf27-5e47397102a8 | in-use    | -          | 10   | DS01        | true     | 5c442589-07b7-4bee-9c68-8a7e5151f3b3 |
+--------------------------------------+-----------+------------+------+-------------+----------+--------------------------------------+
```

4、创建虚拟机

```python
root@jenkins:~# nova boot --flavor 3 --image  f5308779-3ab3-460c-95c9-2791211e640b --nic net-id=a3c9e669-7788-4e7b-913f-57904f307289 --block-device id=378fb335-91bc-4985-a1a1-6b2c3edb40f5,source=volume,dest=volume zd_01
+--------------------------------------+--------------------------------------------------+
| Property                             | Value                                            |
+--------------------------------------+--------------------------------------------------+
| OS-DCF:diskConfig                    | MANUAL                                           |
| OS-EXT-AZ:availability_zone          |                                                  |
| OS-EXT-SRV-ATTR:host                 | -                                                |
| OS-EXT-SRV-ATTR:hypervisor_hostname  | -                                                |
| OS-EXT-SRV-ATTR:instance_name        |                                                  |
| OS-EXT-STS:power_state               | 0                                                |
| OS-EXT-STS:task_state                | scheduling                                       |
| OS-EXT-STS:vm_state                  | building                                         |
| OS-SRV-USG:launched_at               | -                                                |
| OS-SRV-USG:terminated_at             | -                                                |
| accessIPv4                           |                                                  |
| accessIPv6                           |                                                  |
| adminPass                            | PiNzGJnMNxw7                                     |
| config_drive                         |                                                  |
| created                              | 2016-12-08T13:18:25Z                             |
| flavor                               | m1.medium (3)                                    |
| hostId                               |                                                  |
| id                                   | 2090f20f-c157-485a-84ae-252c36756a32             |
| image                                | VSA (f5308779-3ab3-460c-95c9-2791211e640b)       |
| key_name                             | -                                                |
| metadata                             | {}                                               |
| name                                 | zd_01                                            |
| os-extended-volumes:volumes_attached | [{"id": "378fb335-91bc-4985-a1a1-6b2c3edb40f5"}] |
| progress                             | 0                                                |
| security_groups                      | default                                          |
| status                               | BUILD                                            |
| tenant_id                            | 19e87dc3a0eb49549f4a16a042f4ec82                 |
| updated                              | 2016-12-08T13:18:25Z                             |
| user_id                              | 4c3bd81ba5724290b328aaa62e1bfe79                 |
+--------------------------------------+--------------------------------------------------+
```


5、VNC登陆该虚拟机查询虚拟机磁盘信息。

![](https://github.com/XiaoDongZhi/XiaoDongZhi.github.io/blob/master/images/openstack/nova/nova%20boot%20image%20and%20block%20device%20--fdisk%20-l.JPG)