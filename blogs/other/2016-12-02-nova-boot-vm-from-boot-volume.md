---

title: 【OpenStack操作系列 二 】 nova 启动卷启动虚拟机
categories: 云计算
tags: nova
---
*声明：本博客欢迎转发，但请保留原作者信息! 内容系本人学习、研究和总结，如有雷同，实属荣幸！*
 
*新浪微博：[@支支zHi小冬](http://weibo.com/u/1596536485/home?wvr=5)*

*博客地址：[http://xiaodongzhi.github.io/](http://xiaodongzhi.github.io/)*

*联系邮箱：517341003@qq.com*

 
openstack版本信息：ocata

如 [镜像创建卷](http://xiaodongzhi.github.io/openstack%E6%93%8D%E4%BD%9C%E5%91%BD%E4%BB%A4/2016/12/02/OpenStack%E6%93%8D%E4%BD%9C%E7%B3%BB%E5%88%97%E4%B8%80-cinder%E5%88%9B%E5%BB%BAtype-volume-type-%E4%BB%A5%E5%8F%8Atype%E7%9A%84%E4%BD%BF%E7%94%A8)所描述以镜像创建系统卷，改卷可以代替image来创建虚拟机。

一、查询网络信息

```python
stack@jenkins:~$ neutron net-list
+--------------------------------------+---------+---------------------------------------------------------+
| id                                   | name    | subnets                                                 |
+--------------------------------------+---------+---------------------------------------------------------+
| a3c9e669-7788-4e7b-913f-57904f307289 | net101  | 6275d663-2d03-43b6-8376-49ad75a4ce8a 10.101.0.0/24      |
| cc8f8880-728c-49e3-9d68-7c91cd142ecd | private | 56c317bd-57be-4632-9eab-b4572f634365 2001:db8:8000::/64 |
|                                      |         | 26bca241-c876-4d15-a363-00dfa2d7ff87 10.0.0.0/24        |
| fe8cfa17-2cf8-4c6d-83c5-c523441386b3 | public  | 5c5044a8-2e18-4887-8fb3-31137a9c6d40 2001:db8::/64      |
|                                      |         | 8a816c26-c4e0-4bf8-b9e5-d2dca0a6b965 172.24.4.0/24      |
+--------------------------------------+---------+---------------------------------------------------------+
```

二、查询flavor信息

```python
**stack@jenkins:~$ nova flavor-list
+----+-----------+-----------+------+-----------+------+-------+-------------+-----------+
| ID | Name      | Memory_MB | Disk | Ephemeral | Swap | VCPUs | RXTX_Factor | Is_Public |
+----+-----------+-----------+------+-----------+------+-------+-------------+-----------+
| 1  | m1.tiny   | 512       | 1    | 0         |      | 1     | 1.0         | True      |
| 2  | m1.small  | 2048      | 20   | 0         |      | 1     | 1.0         | True      |
| 3  | m1.medium | 4096      | 40   | 0         |      | 2     | 1.0         | True      |
| 4  | m1.large  | 8192      | 80   | 0         |      | 4     | 1.0         | True      |
| 42 | m1.nano   | 64        | 0    | 0         |      | 1     | 1.0         | True      |
| 5  | m1.xlarge | 16384     | 160  | 0         |      | 8     | 1.0         | True      |
| 84 | m1.micro  | 128       | 0    | 0         |      | 1     | 1.0         | True      |
| c1 | cirros256 | 256       | 0    | 0         |      | 1     | 1.0         | True      |
| d1 | ds512M    | 512       | 5    | 0         |      | 1     | 1.0         | True      |
| d2 | ds1G      | 1024      | 10   | 0         |      | 1     | 1.0         | True      |
| d3 | ds2G      | 2048      | 10   | 0         |      | 2     | 1.0         | True      |
| d4 | ds4G      | 4096      | 20   | 0         |      | 4     | 1.0         | True      |
+----+-----------+-----------+------+-----------+------+-------+-------------+-----------+**
```

三、查询启动卷信息

```python
stack@jenkins:~$ cinder list
+--------------------------------------+-----------+------+------+-------------+----------+--------------------------------------+
| ID                                   | Status    | Name | Size | Volume Type | Bootable | Attached to                          |
+--------------------------------------+-----------+------+------+-------------+----------+--------------------------------------+
| 90fc22bf-c45f-4db7-bf27-5e47397102a8 | available | -    | 10   | DS01        | true     |                                      |
+--------------------------------------+-----------+------+------+-------------+----------+--------------------------------------+
```

四、启动卷创建虚拟机

```python
stack@jenkins:~$ nova boot --boot-volume 90fc22bf-c45f-4db7-bf27-5e47397102a8 --flavor 3 --nic net-id=a3c9e669-7788-4e7b-913f-57904f307289 zd001
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
| adminPass                            | xuEADNptC4bk                                     |
| config_drive                         |                                                  |
| created                              | 2016-12-07T16:02:33Z                             |
| flavor                               | m1.medium (3)                                    |
| hostId                               |                                                  |
| id                                   | 418825e9-12f0-4d8e-ba3a-bb52a617cc30             |
| image                                | Attempt to boot from volume - no image supplied  |
| key_name                             | -                                                |
| metadata                             | {}                                               |
| name                                 | zd001                                            |
| os-extended-volumes:volumes_attached | [{"id": "90fc22bf-c45f-4db7-bf27-5e47397102a8"}] |
| progress                             | 0                                                |
| security_groups                      | default                                          |
| status                               | BUILD                                            |
| tenant_id                            | 19e87dc3a0eb49549f4a16a042f4ec82                 |
| updated                              | 2016-12-07T16:02:33Z                             |
| user_id                              | 4c3bd81ba5724290b328aaa62e1bfe79                 |
+--------------------------------------+--------------------------------------------------+

stack@jenkins:~$ nova list
+--------------------------------------+-------+--------+------------+-------------+--------------------+
| ID                                   | Name  | Status | Task State | Power State | Networks           |
+--------------------------------------+-------+--------+------------+-------------+--------------------+
| 5c442589-07b7-4bee-9c68-8a7e5151f3b3 | zd001 | BUILD  | spawning   | NOSTATE     | net101=10.101.0.3  |
+--------------------------------------+-------+--------+------------+-------------+--------------------+

stack@jenkins:~$ nova list
+--------------------------------------+-------+--------+------------+-------------+--------------------+
| ID                                   | Name  | Status | Task State | Power State | Networks           |
+--------------------------------------+-------+--------+------------+-------------+--------------------+
| 5c442589-07b7-4bee-9c68-8a7e5151f3b3 | zd001 | ACTIVE | -          | Running     | net101=10.101.0.3  |
+--------------------------------------+-------+--------+------------+-------------+--------------------+

```

