---
title: Linux Bridge
categories: blog
---

TUN/TAP 设备是一种让用户态程序向内核协议栈注入数据的设备
TUN工作在三层，TAP工作在二层，使用较多的是 TAP 设备。

创建一个TUN/TAP网卡后，Linux会创建一个字符设备，以TAP0设备为例，Linux会创建/dev/tap0设备。用户态程序可以以文件形式读写数据到该字符设备上，完成数据从TAP设备的发送与接收。

*参考：http://en.wikipedia.org/wiki/TUN/TAP*

由于TUN接口工作在三层，典型应用场景为VPN场景。
由于TAP接口工作在二层，可以连接在Bridge上，最常使用的场景为虚拟机提供虚拟网卡。

*参考：http://www.wobblycogs.co.uk/index.php/computing/linux/228-understanding-tun-tap-interfaces*


VETH 设备出现较早，它的作用是反转通讯数据的方向，需要发送的数据会被转换成需要收到的数据重新送入内核网络层进行处理，从而间接的完成数据的注入。



