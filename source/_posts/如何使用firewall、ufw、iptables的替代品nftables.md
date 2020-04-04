---
title: 如何使用firewall、ufw、iptables的替代品nftables
categories:
  - [常用配置]
  - [技巧]
  - [学习]
tags:
date: 2019-08-18 17:25:00
translate_title: how-to-use-nftables-as-an-alternative-to-firewall-ufw-and-iptables
keywords:
description:
top_img: 
comments:
cover: 
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
更改默认设置
```
# update-alternatives --set iptables /usr/sbin/iptables-nft
# update-alternatives --set ip6tables /usr/sbin/ip6tables-nft
# update-alternatives --set arptables /usr/sbin/arptables-nft
# update-alternatives --set ebtables /usr/sbin/ebtables-nft
```
清空当前规则集：
```
# nft flush ruleset
```
添加一个表：
```
# nft add table inet filter
```
添加input、forward和output三个基本链。input和forward的默认策略是drop。output的默认策略是accept。
```
# nft add chain inet filter input { type filter hook input priority 0 \; policy drop \; }
# nft add chain inet filter forward { type filter hook forward priority 0 \; policy drop \; }
# nft add chain inet filter output { type filter hook output priority 0 \; policy accept \; }
```
添加两个与TCP和UDP关联的常规链：
```
# nft add chain inet filter TCP
# nft add chain inet filter UDP
```
related和established的流量会accept：
```
# nft add rule inet filter input ct state related,established accept
```
loopback接口的流量会accept：
```
# nft add rule inet filter input iif lo accept
```
无效的流量会drop：
```
# nft add rule inet filter input ct state invalid drop
```
新的echo请求（ping）会accept：
```
# nft add rule inet filter input ip protocol icmp icmp type echo-request ct state new accept
```
新的UDP流量跳转到UDP链：
```
# nft add rule inet filter input ip protocol udp ct state new jump UDP
```
新的TCP流量跳转到TCP链：
```
# nft add rule inet filter input ip protocol tcp tcp flags \& \(fin\|syn\|rst\|ack\) == syn ct state new jump TCP
```
未由其他规则处理的所有通信会reject：
```
# nft add rule inet filter input ip protocol udp reject
# nft add rule inet filter input ip protocol tcp reject with tcp reset
# nft add rule inet filter input counter reject with icmp type prot-unreachable
```
此时，应决定对传入连接打开哪些端口，这些由TCP和UDP链处理。例如，要打开web服务器的连接端口，添加：
```
# nft add rule inet filter TCP tcp dport 80 accept
```
要打开web服务器HTTPS连接端口443：
```
# nft add rule inet filter TCP tcp dport 443 accept
```
允许SSH连接端口22：
```
# nft add rule inet filter TCP tcp dport 22 accept
```
允许传入DNS请求：
```
# nft add rule inet filter TCP tcp dport 53 accept
# nft add rule inet filter UDP udp dport 53 accept
```
确保更改是永久的（写入到文件）。
```
# nft list ruleset > /etc/nftables.conf
```
参考：https://wiki.archlinux.org/index.php/Nftables_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)