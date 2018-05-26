---
layout: post
title: 【OpenStacks实践系列 一 】cinder创建type（volume-type）以及type的使用
categories: OpenStacks实践
---
*声明：本博客欢迎转发，但请保留原作者信息! 内容系本人学习、研究和总结，如有雷同，实属荣幸！*
 
*新浪微博：[@支支zHi小冬](http://weibo.com/u/1596536485/home?wvr=5)*

*博客地址：[http://xiaodongzhi.github.io/](http://xiaodongzhi.github.io/)*

*联系邮箱：517341003@qq.com*

 
openstack版本信息：ocata

一、创建volume-type

```python
stack@jenkins:~$ cinder type-create DS01
+--------------------------------------+------+-------------+-----------+
| ID                                   | Name | Description | Is_Public |
+--------------------------------------+------+-------------+-----------+
| f29e646d-c6c0-4f4c-b3b0-718a6c8a563b | DS01 | -           | True      |
+--------------------------------------+------+-------------+-----------+
```

二、配置volume-type关联的后端存储名称

```python
stack@jenkins:~$ cinder type-key DS01 set volume_backend_name=DS01

```

三、创建磁盘时，指定volume-type

```python
stack@jenkins:/etc/cinder$ cinder create --image-id f5308779-3ab3-460c-95c9-2791211e640b 10 --volume-type DS01
+--------------------------------+--------------------------------------+
| Property                       | Value                                |
+--------------------------------+--------------------------------------+
| attachments                    | []                                   |
| availability_zone              | nova                                 |
| bootable                       | false                                |
| consistencygroup_id            | None                                 |
| created_at                     | 2016-12-06T08:52:32.000000           |
| description                    | None                                 |
| encrypted                      | False                                |
| id                             | c590ca7b-e756-4038-9998-c87fd0374113 |
| metadata                       | {}                                   |
| migration_status               | None                                 |
| multiattach                    | False                                |
| name                           | None                                 |
| os-vol-host-attr:host          | None                                 |
| os-vol-mig-status-attr:migstat | None                                 |
| os-vol-mig-status-attr:name_id | None                                 |
| os-vol-tenant-attr:tenant_id   | 19e87dc3a0eb49549f4a16a042f4ec82     |
| replication_status             | disabled                             |
| size                           | 10                                   |
| snapshot_id                    | None                                 |
| source_volid                   | None                                 |
| status                         | creating                             |
| updated_at                     | None                                 |
| user_id                        | 4c3bd81ba5724290b328aaa62e1bfe79     |
| volume_type                    | DS01                                 |
+--------------------------------+--------------------------------------+
stack@jenkins:/etc/cinder$ cinder list
+--------------------------------------+----------+------+------+-------------+----------+-------------+
| ID                                   | Status   | Name | Size | Volume Type | Bootable | Attached to |
+--------------------------------------+----------+------+------+-------------+----------+-------------+
| c590ca7b-e756-4038-9998-c87fd0374113 | creating | -    | 10   | DS01        | false    |             |
+--------------------------------------+----------+------+------+-------------+----------+-------------+
stack@jenkins:/etc/cinder$ cinder list
+--------------------------------------+-----------+------+------+-------------+----------+-------------+
| ID                                   | Status    | Name | Size | Volume Type | Bootable | Attached to |
+--------------------------------------+-----------+------+------+-------------+----------+-------------+
| c590ca7b-e756-4038-9998-c87fd0374113 | available | -    | 10   | DS01        | true     |             |
+--------------------------------------+-----------+------+------+-------------+----------+-------------+
```
