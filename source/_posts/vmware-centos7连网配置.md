---
title: vmware_centos7连网配置
date: 2018-03-09 16:35:58
tags: LIUNX
---

vmware_centos7连网配置

author：李文良
<!-- more -->

首发于博客:
勘误:https://github.com/icngor/icngor.github.io/issues/2
---
vmware_centos7连网配置
## 1. 使用桥接模式(相当于局域网一台真实主机)
- 虚拟机->设置->网络适配器->网络连接->桥接模式
- 编辑->虚拟网络编辑器->vMnet信息->桥接模式->选择可以的网卡

## 2. Centos7配置
IP地址配置
```
[root@localhost ~]# vi /etc/sysconfig/network-scripts/ifcfg-ens33

TYPE=Ethernet
PROXY_METHOD=static # 静态IP
BROWSER_ONLY=no
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=aad1e567-65cc-4144-9dc3-8de0d2aac6bd
DEVICE=ens33
ONBOOT=yes # 系统启动时激活网卡
IPADDR=192.168.0.249
NETMASK=255.255.255.0
GATEWAY=192.168.0.1
```
dns配置 & 重启网络
```
[root@localhost ~]# vi /etc/resolv.conf
nameserver 8.8.8.8

[root@localhost ~]# systemctl restart network
```