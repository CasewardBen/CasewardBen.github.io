---
layout: post
title: linux之centos版本网络配置问题
category: linux
tages: [centos]
---

不同版本的centos的网络脚本文件名称存在一定的区别，
一般有 ifcfg-eth* 和 ifcfg-ens* 两种，具体视情况而定
进入network-scripts目录中执行查看
```bash
cd /etc/sysconfig/network-scripts
ls -a
```
查看存在的 ifcfg-eth* 或者 ifcfg-ens* 网络脚本文件内容
```bash
cat ifcfg-ens33
```
编辑网络脚本文件
```bash
vi ifcfg-ens33
```
修改内容如下
```bash
ONBOOT = yes
BOOTPROTO = dhcp
```
查看IP地址、掩码和网关
VMware虚拟机上
[编辑]->[虚拟网络编辑器]->[VMnet8]->[NAT设置]
修改resolv.conf文件
```bash
vi /etc/resolv.conf
```
插入
```bash
nameserver 192.168.1.2[你查找的ip地址]
```
重启网络服务
```bash
service network restart
```
验证网络，保证主机已连入网络
```bash
ping www.baidu.com
```
ctr+c退出
