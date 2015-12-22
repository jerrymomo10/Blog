title: Linux下固定mac和ip绑定方式
tags: [Linux]
----------
在实验室帮师弟的Ubuntu14.04解决上网问题时发现了Ubuntu下固定mac和ip上网方式时的一些步骤，有些教程是不全面的甚至是错误的，所以在这里总结一下。
Linux有一些命令可以简单的设置固定ip和mac，但是系统重启以后就失效的方法，这里就不提了，介绍的是永久的修改方法。


----------

 - 

IP的固定修改/etc/network/interfaces
 将DHCP屏蔽 
> iface eth0 inet dhcp

 添加静态ip有关的参数
>   iface eth0 inet static
    address 192.168.0.10
    netmask 255.255.255.0
    gateway 192.168.0.1
    

 

 - 设置DNS修改/etc/resolv.conf
> nameserver 202.96.134.133
  nameserver 202.106.0.20

虽然设置了固定的DNS，但是每次重启DNS都会被重写，所以也要修改/etc/resolvconf/resolv.conf.d/base这个文件为
>nameserver 202.96.134.133
 nameserver 202.106.0.20
 

 - 固定mac设定，在/etc/rc.d/rc.local中添加：

>ifconfig eth0 down
ifconfig eth0 hw ether 00:0C:18:EF:FF:ED
ifconfig eth0 up

 - 然后执行重启网卡设置：
> sudo ifdown eth0
 sudo ifup eth0 
  ifconfig -a
 
 - 修改默认网关，至关重要，最后发现的问题

> route add default gw 192.168.2.1 

但是每次重启系统，都要执行这条命令，最好把它写入/etc/rc.local 
> route add gw 192.168.2.254 eth0


