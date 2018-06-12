单虚拟机     
配置eth0:0,ip地址100.112.76.193   
eth0的IP地址100.112.76.192   

```shell
[root@kwep000100059 ~]# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 100.112.76.192  netmask 255.255.254.0  broadcast 100.112.77.255
        inet6 fe80::2a6e:d4ff:fe89:3c1c  prefixlen 64  scopeid 0x20<link>
        ether 28:6e:d4:89:3c:1c  txqueuelen 1000  (Ethernet)
        RX packets 2829  bytes 178325 (174.1 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1522  bytes 251253 (245.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth0:0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 100.112.76.193  netmask 255.255.255.0  broadcast 100.112.76.255
        ether 28:6e:d4:89:3c:1c  txqueuelen 1000  (Ethernet)

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 0  (Local Loopback)
        RX packets 765  bytes 118568 (115.7 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 765  bytes 118568 (115.7 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

virbr0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 52:54:00:c2:a7:45  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
