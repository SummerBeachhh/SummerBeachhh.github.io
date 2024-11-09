---
title: 【转载】 通过openwrt的IPV6中继(也叫ipv6透传)，使后端设备获得原生ipv6公网地址-OPENWRT专版-恩山无线论坛
date: 2024-11-09
category: 网络
tags: [路由器]
---

_本帖最后由 bennyzhou 于 2022-4-6 23:56 编辑_

实在忍不住了。看到都还在折腾NAT6。其实IPv6直接可以中继的。LAN端设备直接能拿原生公网ipv6地址不是更好？而且设置超级简单

纯属瞎折腾的NAT6方案，搞得好像很复杂，其实性能也很差。除非上游只能给你/128地址，那只能nat6。（我从没看到过，至少也能分配/64的）

2020已经有人验证中继方案可用于校园网。见下面的帖子。只是当时的固件还不能用纯鼠标操作
https://www.right.com.cn/FORUM/thread-4054136-1-1.html

简单说就是
中继（透传，厂商主流方案）适合路由器拿不到PD，只能拿到/64地址的情况。如校园网
客户端接入op后，同时获得ipv4和ipv6
ipv4为私有地址，op路由器分配，流量需nat改包后转发，效率低，速度慢。外部访问需端口映射
ipv6为公网地址，通过wan6中继到lan，流量只需直接路由转发，效率高，速度高。外部访问不需端口映射，只需打开防火墙规则

NAT6（恩山流行方案）
客户端接入op后，同时获得ipv4和ipv6
ipv4为私有地址，op路由器分配，流量需nat改包后转发，效率低，速度慢。外部访问需端口映射
ipv6为私有地址，op路由器分配，流量需nat6改包后转发。因ipv6地址更长，所以效率更低，速度更慢。外部访问需端口映射

很多家用路由器原生有这功能，如网件，华硕。他们叫 ipv6 passthru（ipv6透传），实际是一样的功能

原生地址是从公网直接可以访问的（注意：op上wan zone的防火墙规则还是要开的）

**nat6强烈不推荐，因为ipv6地址比较长，nat后极度消耗路由器资源，速度上不去的。而原生ipv6不存在地址转换，不占资源，极快**

正常如果WAN端支持IPV6-PD的话，LAN端打开DHCPv6 server端即可。但是如果WAN端只能分配/64的地址的话，就需要通过中继的方式，把WAN端的ipv6穿到LAN

（以下为英文原版op，版本21.02.2）_因为原版op的网页设置里有master选项直接打勾就行了。中文改版op有的没有，要通过ssh改conf。你说哪个容易？_
首先新建WAN6，协议dhcpv6 client，firewall zone改成wan，确保wan6能拿到v6地址（若已有wan6接口请忽略前面这步）

1. DHCP server，点设置DHCP

Ignore interface打勾

2. 在Ipv6 settings，

Designated master打勾

3. RA-Service，

DHCPv6-Service，

NDP-Proxy这3个下拉框全部改成relay mode

4. Learn routes打勾

5. 点save

然后到LAN

1. DHCP server，在Ipv6 settings中，RA-Service改成relay mode, DHCPv6-Service改成hybrid, NDP-Proxy改成relay mode
2. Local IPv6 DNS server打勾
3. Learn routes打勾
4. 点save

这样LAN端就能通过wan端拿ipv6的原生地址了。ipv6防火墙也可以设置规则

原理

1. WAN6端Designated master是告诉系统这边是上游
2. LAN端通过WAN端的NDP-proxy拿到原生v6地址
3. LAN端的dhcp是为了告知dns v6解析服务器地址的
4. RA-Service是告知LAN端拿到v6地址的主机，上级路由服务器是哪个

正常联通，移动，电信，都会给PD。所以不需要中继。但是我前段时间在一个地方。路由器WAN口只能拿到/64的地址。没法向下分配。所以才查了op英文论坛搞出来的。实测可用

另外，op原版21.02，官方bug报告说，开nat offloading会造成ipv6断流。需关闭加速。至今没解决

nat加速需关闭。最新版op 21，否则ipv6时常断流。这个问题。至今21.02.2还没解决

原文

**Known issues**Some IPv6 packets are dropped when software flow offloading is used: [FS#3373](https://bugs.openwrt.org/index.php?do=details&task_id=3373)
As a workaround, do not activate software flow offloading, it is deactivate by default.

中文改版中，有的没有Designated master这个选项，请参考下面中文资料，修改conf文件

参考中文资料

https://l2dy.sourceforge.io/2021/05/11/openwrt-ipv6-relay.html

_原贴地址https://www.right.com.cn/forum/thread-8217538-1-1.html_
