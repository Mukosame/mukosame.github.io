---
layout: post
title: 主机组（host aggregate）
categories: OpenStack
---

主机组面向的是管理员;
Available Zone面向的是所有用户

Available Zone下包含若干主机组。

一个主机可以存在多个主机组内
一个主机组可以包含多个key-value对
同一个key-value对可以被分配到多个主机组上



----------

用户创建的虚拟机的时候选择的availablitity-zone。
nova-schedule将该虚拟机调度到属于对应availability-zone下的主机组下的主机。如`host1`。
此时如果将`host1`从原主机组删除，将host1添加到属于另外一个availability-zone的主机组下，如新availability-zone为`Xi'An`，则原虚拟机的az属性也将更新为`Xi'An`.


----------



openstack官网（http://docs.openstack.org/trunk/config-reference/content/section_compute-scheduler.html）
关于nova schedule的说明中有个关于主机组的例子很形象：




Example: Specify compute hosts with SSDs
This example configures the Compute service to enable users to request nodes that have solid-state drives (SSDs). You create a fast-io host aggregate in the nova availability zone and you add the ssd=true key-value pair to the aggregate. Then, you add the node1, and node2 compute nodes to it.

```
$ nova aggregate-create fast-io nova
+----+---------+-------------------+-------+----------+
| Id | Name    | Availability Zone | Hosts | Metadata |
+----+---------+-------------------+-------+----------+
| 1  | fast-io | nova              |       |          |
+----+---------+-------------------+-------+----------+

$ nova aggregate-set-metadata 1 ssd=true
+----+---------+-------------------+-------+-------------------+
| Id | Name    | Availability Zone | Hosts | Metadata          |
+----+---------+-------------------+-------+-------------------+
| 1  | fast-io | nova  			   | []    | {u'ssd': u'true'} |
+----+---------+-------------------+-------+-------------------+

$ nova aggregate-add-host 1 node1
+----+---------+-------------------+-----------+--------------------+
| Id | Name    | Availability Zone | Hosts     | Metadata           |
+----+---------+-------------------+------------+-------------------+
| 1  | fast-io | nova  			   | [u'node1']| {u'ssd': u'true'}  |
+----+---------+-------------------+------------+-------------------+

$ nova aggregate-add-host 1 node2
+----+---------+-------------------+---------------------+-------------------+
| Id | Name    | Availability Zone | Hosts               | Metadata          |
+----+---------+-------------------+----------------------+-------------------+
| 1  | fast-io | nova              | [u'node1', u'node2']| {u'ssd': u'true'}  |
+----+---------+-------------------+----------------------+-------------------+
Use the nova flavor-create command to create the ssd.large flavor called with an ID of 6, 8 GB of RAM, 80 GB root disk, and four vCPUs.

$ nova flavor-create ssd.large 6 8192 80 4
+----+-----------+-----------+------+-----------+------+-------+-------------+-----------+-------------+
| ID | Name      | Memory_MB | Disk | Ephemeral | Swap | VCPUs | RXTX_Factor   | Is_Public | extra_specs |
+----+-----------+-----------+------+-----------+------+-------+-------------+-----------+-------------+
| 6  | ssd.large | 8192   | 80      | 0         |      | 4     |1              | True      | {}          |
+----+-----------+-----------+------+-----------+------+-------+-------------+-----------+-------------+
```

Once the flavor is created, specify one or more key-value pairs that match the key-value pairs on the host aggregates. In this case, that is the ssd=true key-value pair. Setting a key-value pair on a flavor is done using the nova flavor-key command.

```    
$ nova flavor-key ssd.large set  ssd=true
```

Once it is set, you should see the extra_specs property of the ssd.large flavor populated with a key of ssd and a corresponding value of true.

```
$ nova flavor-show ssd.large
+----------------------------+-------------------+
| Property                   | Value             |
+----------------------------+-------------------+
| OS-FLV-DISABLED:disabled   | False             |
| OS-FLV-EXT-DATA:ephemeral  | 0                 |
| disk                       | 80                |
| extra_specs                | {u'ssd': u'true'} |
| id                         | 6                 |
| name                       | ssd.large         |
| os-flavor-access:is_public | True              |
| ram                        | 8192              |
| rxtx_factor                | 1.0               |
| swap                       |                   |
| vcpus                      | 4                 |
+----------------------------+-------------------+
```

Now, when a user requests an instance with the ssd.large flavor, the scheduler only considers hosts with the ssd=true key-value pair. In this example, these are node1 and node2.