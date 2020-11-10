---
title: neutron之network初探
categories: 云计算
---
可能由于工作历史的原因，对网络一如既往的热情，openstack环境好之后，迫不及待的先实践下网络部分。


首先创建一个叫private-network4的网络。
在该网络下创建private-network4-subnet1的子网，网段为172.24.8.0/24，该子网使能了dhcp。

接下来在router上创建一个port，子网选择private-network4-subnet1,ip选172.24.8.1。

首先简单观察下日志。这段时间发生的事情。


对于子网，如果配置使能dhcp，则在router上创建该子网的port的时候，neutron会自动为该子网创建一个属主"network:router_interface"的port和一个属主为"network:dhcp"的port。


如果子网不使能dhcp，则在router上创建该子网的port的时候，neutron只会为该子网创建一个属主为"network:router_interface"的port。

----------

VXLAN:

br-tun为neutron里面ovs创建支持VXLAN的网桥。
该网桥通过openflow流表策略实现vlan和tun_id的匹配，并将报文从对应的接口发出去：
1、匹配从br-int进入br-tun的vlan报文，将该vlan报文vlan标签干掉，根据对应的tun_id从对应的网口转发出去。
2、从br-tun的其他接口（该接口与其他节点的br-tun网口相连）收到报文后，匹配该报文的tun_id后，将vlan标签的值修改为对应的vlan值，从br-int转发出去。