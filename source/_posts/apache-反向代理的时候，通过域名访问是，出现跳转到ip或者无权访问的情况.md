---
title: apache 反向代理的时候，通过域名访问是，出现跳转到ip或者无权访问的情况
categories:
  - [技巧]
tags:
date: 2019-09-19 21:28:00
translate_title: when-apache-reverse-proxy-is-accessed-through-domain-name-there-is-a-jump-to-ip-or-no-access
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
在配置反向代理的时候加上一行：

ProxyPreserveHost On

例如：
ProxyPass / http://202.112.0.1/
ProxyPassReverse / http://202.112.0.1/
ProxyPreserveHost On #在这里加上就行