---
title: Fedora中设置开机脚本，并配置he-ipv6
categories:
  - [常用配置]
  - [技巧]
tags:
date: 2019-08-13 17:22:00
translate_title: set-up-boot-script-in-fedora-and-configure-heipv6
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
```
sudo touch /etc/rc.d/rc.local
sudo nano /etc/rc.d/rc.local
```
```
#!/bin/bash
sh /mycmd/getipv6.sh
```
```
chmod +x /etc/rc.d/rc.local
sudo nano /usr/lib/systemd/system/rc-local.service
```
```
[Install]
WantedBy=multi-user.target
sudo systemctl enable rc-local.service
```