---
layout: post
title: 【OpenStack实践系列 四 】nova 给虚拟机添加删除网卡
categories: blog
---
*声明：本博客欢迎转发，但请保留原作者信息! 内容系本人学习、研究和总结，如有雷同，实属荣幸！*
 
*新浪微博：[@支支zHi小冬](http://weibo.com/u/1596536485/home?wvr=5)*

*博客地址：[http://xiaodongzhi.github.io/](http://xiaodongzhi.github.io/)*

*联系邮箱：517341003@qq.com*

 
openstack版本信息：ocata


----------

## 给虚拟机添加网卡 ##

1、创建网卡

```python
stack@jenkins:~/logs$ neutron port-create net101
Created a new port:
+-----------------------+------------------------------------------------------------------------------------+
| Field                 | Value                                                                              |
+-----------------------+------------------------------------------------------------------------------------+
| admin_state_up        | True                                                                               |
| allowed_address_pairs |                                                                                    |
| binding:host_id       |                                                                                    |
| binding:profile       | {}                                                                                 |
| binding:vif_details   | {}                                                                                 |
| binding:vif_type      | unbound                                                                            |
| binding:vnic_type     | normal                                                                             |
| created_at            | 2016-12-08T09:32:59Z                                                               |
| description           |                                                                                    |
| device_id             |                                                                                    |
| device_owner          |                                                                                    |
| extra_dhcp_opts       |                                                                                    |
| fixed_ips             | {"subnet_id": "6275d663-2d03-43b6-8376-49ad75a4ce8a", "ip_address": "10.101.0.19"} |
| id                    | 7decda09-38b7-4d27-af9d-b9374ef4f067                                               |
| mac_address           | fa:16:3e:26:75:2d                                                                  |
| name                  |                                                                                    |
| network_id            | a3c9e669-7788-4e7b-913f-57904f307289                                               |
| port_security_enabled | True                                                                               |
| project_id            | 19e87dc3a0eb49549f4a16a042f4ec82                                                   |
| revision_number       | 5                                                                                  |
| security_groups       | 26ebd828-daa8-4dbf-b26f-0dc86981c970                                               |
| status                | DOWN                                                                               |
| tenant_id             | 19e87dc3a0eb49549f4a16a042f4ec82                                                   |
| updated_at            | 2016-12-08T09:33:00Z                                                               |
+-----------------------+------------------------------------------------------------------------------------+

```

2、将网卡挂载到虚拟机上。

```python
stack@jenkins:~/logs$ nova interface-attach --port-id  7decda09-38b7-4d27-af9d-b9374ef4f067   158fb48e-960b-4b3b-880a-d251c2318bd3
```

3、查看虚拟机详情中的网卡信息，可见在“net101 network”中增加了“10.101.0.19”的IP地址，即虚拟机网卡挂载成功。

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
| net101 network                       | 10.101.0.10, 10.101.0.9, 10.101.0.17, 10.101.0.180, 10.101.0.16, 10.101.0.19                                                                     |
| os-extended-volumes:volumes_attached | [{"id": "00295e6b-d27d-4144-9311-32d1342e78fc"}, {"id": "bce03ec0-6ca1-4273-83b8-93d311b4a799"}, {"id": "f21e49ef-5f59-491b-960f-2e91646bf6d2"}] |
| progress                             | 0                                                                                                                                                |
| security_groups                      | default                                                                                                                                          |
| status                               | ACTIVE                                                                                                                                           |
| tenant_id                            | 19e87dc3a0eb49549f4a16a042f4ec82                                                                                                                 |
| updated                              | 2016-12-07T13:30:34Z                                                                                                                             |
| user_id                              | 4c3bd81ba5724290b328aaa62e1bfe79                                                                                                                 |
+--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
```


----------


## 给虚拟机卸载网卡 ##

1、卸载磁盘

```python
stack@jenkins:~/logs$ nova interface-detach 158fb48e-960b-4b3b-880a-d251c2318bd3 7decda09-38b7-4d27-af9d-b9374ef4f067
```

2、查看虚拟机网卡信息，即可见“10.101.0.19”的IP地址已经不存在，说明已经卸载成功：

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
| net101 network                       | 10.101.0.10, 10.101.0.9, 10.101.0.17, 10.101.0.180, 10.101.0.16                                                                                  |
| os-extended-volumes:volumes_attached | [{"id": "00295e6b-d27d-4144-9311-32d1342e78fc"}, {"id": "bce03ec0-6ca1-4273-83b8-93d311b4a799"}, {"id": "f21e49ef-5f59-491b-960f-2e91646bf6d2"}] |
| progress                             | 0                                                                                                                                                |
| security_groups                      | default                                                                                                                                          |
| status                               | ACTIVE                                                                                                                                           |
| tenant_id                            | 19e87dc3a0eb49549f4a16a042f4ec82                                                                                                                 |
| updated                              | 2016-12-07T13:30:34Z                                                                                                                             |
| user_id                              | 4c3bd81ba5724290b328aaa62e1bfe79                                                                                                                 |
+--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
```
