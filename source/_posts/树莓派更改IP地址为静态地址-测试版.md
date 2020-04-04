---
title: 树莓派更改IP地址为静态地址 --测试版
categories:
  - [技巧]
tags:
date: 2020-02-13 21:54:00
translate_title: raspberry-pie-change-ip-address-to-static-address
keywords:
description:
top_img: /2020/02/13/raspberry-pie-change-ip-address-to-static-address/pi_debian.png
comments:
cover: /2020/02/13/raspberry-pie-change-ip-address-to-static-address/pi_debian.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
树莓派更改IP地址为静态地址，方便端口映射

首先，官方镜像的树莓派是基于Debian系统，所以命令会宇它有类似
<img src="pi_debian.png" width="100%" alt="pi_debian">

那么更改IP也是一样

***注意：未经过测试可能出现连不上网的情况***
恢复方法
```
sudo rm -rf /etc/dhcpcd.conf
sudo cp /etc/dhcpcd.conf.bak /etc/dhcpcd.conf
systemctl restart dhcpcd
```


----------


一：明确网络连接方式，及网卡名称
如通过网线连接会出现eth0网卡或ens0网卡
WiFi连接则出现wlan0等
```
ifconfig
```
如果提示```command not found```则需要安装```net-tools```包
``` 
sudo apt-get install net-tools -y
```
<img src="pi_ifconfig.png" width="100%" alt="pi_ifconfig">
第一个为Docker虚拟网卡不管
第二个则为真实网卡
二：修改配置
复制来备份配置文件
```
sudo cp /etc/dhcpcd.conf /etc/dhcpcd.conf.bak
```
```
sudo nano /etc/dhcpcd.conf
```
如果提示```command not found```则需要安装```nano```包,当然你也可以用vi或者vim
``` 
sudo apt-get install nano -y
```
<img src="pi_dhcpcd.png" width="100%" alt="pi_dhcpcd">
再末尾添加配置
请勿直接复制
```
interface eth0
static ip_address=192.168.0.10/24	
static routers=192.168.0.1
static domain_name_servers=192.168.0.1 8.8.8.8
```
或者
```
interface wlan0
static ip_address=192.168.0.10/24	
static routers=192.168.0.1
static domain_name_servers=192.168.0.1 8.8.8.8
```
eth0或wlan0为你真实网卡的名称
ip_address就是静态IP , 后面要接/24 , 最好往后设一点
routers是网关
static domain_name_servers是DNS
Ctrl+o 回车保存 Ctrl+x退出编辑，当然你用vi或者vim也可以
```
systemctl restart dhcpcd
```
重启dhcpcd网络
三：登录路由器查看IP地址

  
  
  