---

title: 【OpenStack实践系列 三 】nova 给虚拟机挂载磁盘
categories: 云计算
tags: nova
---
*声明：本博客欢迎转发，但请保留原作者信息! 内容系本人学习、研究和总结，如有雷同，实属荣幸！*
 
*新浪微博：[@支支zHi小冬](http://weibo.com/u/1596536485/home?wvr=5)*

*博客地址：[http://xiaodongzhi.github.io/](http://xiaodongzhi.github.io/)*

*联系邮箱：517341003@qq.com*

 
openstack版本信息：ocata

给虚拟机挂载磁盘

1、创建磁盘

```python
stack@jenkins:~/logs$ cinder create --name zd_disk_01 10
+--------------------------------+--------------------------------------+
| Property                       | Value                                |
+--------------------------------+--------------------------------------+
| attachments                    | []                                   |
| availability_zone              | nova                                 |
| bootable                       | false                                |
| consistencygroup_id            | None                                 |
| created_at                     | 2016-12-08T08:34:50.000000           |
| description                    | None                                 |
| encrypted                      | False                                |
| id                             | ef028c34-1153-40a2-a518-08f30776f469 |
| metadata                       | {}                                   |
| migration_status               | None                                 |
| multiattach                    | False                                |
| name                           | zd_disk_01                           |
| os-vol-host-attr:host          | None                                 |
| os-vol-mig-status-attr:migstat | None                                 |
| os-vol-mig-status-attr:name_id | None                                 |
| os-vol-tenant-attr:tenant_id   | 19e87dc3a0eb49549f4a16a042f4ec82     |
| replication_status             | disabled                             |
| size                           | 10                                   |
| snapshot_id                    | None                                 |
| source_volid                   | None                                 |
| status                         | error                                |
| updated_at                     | 2016-12-08T08:34:50.000000           |
| user_id                        | 4c3bd81ba5724290b328aaa62e1bfe79     |
| volume_type                    | lvmdriver-1                          |
+--------------------------------+--------------------------------------+

```

2、将该磁盘挂载到虚拟机上

```python
stack@jenkins:~/logs$ nova volume-attach 158fb48e-960b-4b3b-880a-d251c2318bd3 378fb335-91bc-4985-a1a1-6b2c3edb40f5
+----------+--------------------------------------+
| Property | Value                                |
+----------+--------------------------------------+
| device   | /dev/sdd                             |
| id       | 378fb335-91bc-4985-a1a1-6b2c3edb40f5 |
| serverId | 158fb48e-960b-4b3b-880a-d251c2318bd3 |
| volumeId | 378fb335-91bc-4985-a1a1-6b2c3edb40f5 |
+----------+--------------------------------------+

```

3、查看虚拟机详情中的磁盘信息

```python
stack@jenkins:~/logs$ nova show 158fb48e-960b-4b3b-880a-d251c2318bd3
+--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Property                             | Value                                                                                                                                                                                            |
+--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| OS-DCF:diskConfig                    | MANUAL                                                                                                                                                                                           |
| OS-EXT-AZ:availability_zone          | nova                                                                                                                                                                                             |
| OS-EXT-SRV-ATTR:host                 | jenkins                                                                                                                                                                                          |
| OS-EXT-SRV-ATTR:hypervisor_hostname  | 3800067A@Cluster001                                                                                                                                                                              |
| OS-EXT-SRV-ATTR:instance_name        | instance-0000002c                                                                                                                                                                                |
| OS-EXT-STS:power_state               | 1                                                                                                                                                                                                |
| OS-EXT-STS:task_state                | -                                                                                                                                                                                                |
| OS-EXT-STS:vm_state                  | active                                                                                                                                                                                           |
| OS-SRV-USG:launched_at               | 2016-12-07T13:30:34.000000                                                                                                                                                                       |
| OS-SRV-USG:terminated_at             | -                                                                                                                                                                                                |
| accessIPv4                           |                                                                                                                                                                                                  |
| accessIPv6                           |                                                                                                                                                                                                  |
| config_drive                         |                                                                                                                                                                                                  |
| created                              | 2016-12-07T13:27:25Z                                                                                                                                                                             |
| flavor                               | m1.medium (3)                                                                                                                                                                                    |
| hostId                               | 1f157401837a8e247bbc4537b6455aebf05b0c6bf86d41eb8f8fca09                                                                                                                                         |
| id                                   | 158fb48e-960b-4b3b-880a-d251c2318bd3                                                                                                                                                             |
| image                                | VSA (f5308779-3ab3-460c-95c9-2791211e640b)                                                                                                                                                       |
| key_name                             | -                                                                                                                                                                                                |
| metadata                             | {}                                                                                                                                                                                               |
| name                                 | zd409                                                                                                                                                                                            |
| net101 network                       | 10.101.0.10, 10.101.0.9, 10.101.0.17, 10.101.0.180                                                                                                                                               |
| os-extended-volumes:volumes_attached | [{"id": "00295e6b-d27d-4144-9311-32d1342e78fc"}, **{"id": "378fb335-91bc-4985-a1a1-6b2c3edb40f5"}**, {"id": "bce03ec0-6ca1-4273-83b8-93d311b4a799"}, {"id": "f21e49ef-5f59-491b-960f-2e91646bf6d2"}] |
| progress                             | 0                                                                                                                                                                                                |
| security_groups                      | default                                                                                                                                                                                          |
| status                               | ACTIVE                                                                                                                                                                                           |
| tenant_id                            | 19e87dc3a0eb49549f4a16a042f4ec82                                                                                                                                                                 |
| updated                              | 2016-12-07T13:30:34Z                                                                                                                                                                             |
| user_id                              | 4c3bd81ba5724290b328aaa62e1bfe79                                                                                                                                                                 |
+--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

```



----------

给虚拟机卸载磁盘

1、卸载磁盘

```python
stack@jenkins:~/logs$ nova volume-detach 158fb48e-960b-4b3b-880a-d251c2318bd3 378fb335-91bc-4985-a1a1-6b2c3edb40f5
```

2、查看虚拟机磁盘信息：

```python
stack@jenkins:~/logs$ nova show 158fb48e-960b-4b3b-880a-d251c2318bd3
+--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| Property                             | Value                                                                                                                                            |
+--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| OS-DCF:diskConfig                    | MANUAL                                                                                                                                           |
| OS-EXT-AZ:availability_zone          | nova                                                                                                                                             |
| OS-EXT-SRV-ATTR:host                 | jenkins                                                                                                                                          |
| OS-EXT-SRV-ATTR:hypervisor_hostname  | 3800067A@Cluster001                                                                                                                              |
| OS-EXT-SRV-ATTR:instance_name        | instance-0000002c                                                                                                                                |
| OS-EXT-STS:power_state               | 1                                                                                                                                                |
| OS-EXT-STS:task_state                | -                                                                                                                                                |
| OS-EXT-STS:vm_state                  | active                                                                                                                                           |
| OS-SRV-USG:launched_at               | 2016-12-07T13:30:34.000000                                                                                                                       |
| OS-SRV-USG:terminated_at             | -                                                                                                                                                |
| accessIPv4                           |                                                                                                                                                  |
| accessIPv6                           |                                                                                                                                                  |
| config_drive                         |                                                                                                                                                  |
| created                              | 2016-12-07T13:27:25Z                                                                                                                             |
| flavor                               | m1.medium (3)                                                                                                                                    |
| hostId                               | 1f157401837a8e247bbc4537b6455aebf05b0c6bf86d41eb8f8fca09                                                                                         |
| id                                   | 158fb48e-960b-4b3b-880a-d251c2318bd3                                                                                                             |
| image                                | VSA (f5308779-3ab3-460c-95c9-2791211e640b)                                                                                                       |
| key_name                             | -                                                                                                                                                |
| metadata                             | {}                                                                                                                                               |
| name                                 | zd409                                                                                                                                            |
| net101 network                       | 10.101.0.10, 10.101.0.9, 10.101.0.17, 10.101.0.180                                                                                               |
| os-extended-volumes:volumes_attached | [{"id": "00295e6b-d27d-4144-9311-32d1342e78fc"}, {"id": "bce03ec0-6ca1-4273-83b8-93d311b4a799"}, {"id": "f21e49ef-5f59-491b-960f-2e91646bf6d2"}] |
| progress                             | 0                                                                                                                                                |
| security_groups                      | default                                                                                                                                          |
| status                               | ACTIVE                                                                                                                                           |
| tenant_id                            | 19e87dc3a0eb49549f4a16a042f4ec82                                                                                                                 |
| updated                              | 2016-12-07T13:30:34Z                                                                                                                             |
| user_id                              | 4c3bd81ba5724290b328aaa62e1bfe79                                                                                                                 |
+--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+

```
