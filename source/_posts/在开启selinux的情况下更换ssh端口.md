---
title: 在开启selinux的情况下更换ssh端口
categories:
  - [常用配置]
  - [技巧]
tags:
date: 2019-08-13 17:22:00
translate_title: replacement-of-ssh-port-with-selinux-enabled
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
semanage port -a -t ssh_port_t -p tcp 22122