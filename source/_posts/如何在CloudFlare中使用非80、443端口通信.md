---
title: 如何在CloudFlare中使用非80、443端口通信
categories:
  - [技巧]
tags:
  - [Cloudflare]
  - [通信]
  - [80]
  - [443]
date: 2019-08-05 17:17:34
translate_title: how-to-use-non80-and-443-ports-to-communicate-in-cloudflare
keywords:
description:
top_img: /2019/08/05/how-to-use-non80-and-443-ports-to-communicate-in-cloudflare/port.png
comments:
cover: /2019/08/05/how-to-use-non80-and-443-ports-to-communicate-in-cloudflare/port.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
网页使用80，443端口
Cloudflare几乎可以代理所有TCP端口。支持两种代理：
应用程序级别（第7层）HTTP代理，频谱，传输级别（第4层）TCP代理
HTTP代理
Cloudflare可以代理通过下面列出的HTTP / HTTPS端口的流量。

如果您的流量位于不同的端口，则可以将其作为我们不代理的内容添加为Cloudflare DNS区域文件中的记录（灰色云=没有Cloudflare代理或记录缓存）。

Cloudflare支持的HTTP端口是：
80 
8080 
8880 
2052 
2082 
2086 
2095

Cloudflare支持的HTTPs端口是：
443 
2053 
2083 
2087 
2096 
8443

对于Pro计划及更高版本，您可以使用WAF规则ID 100015阻止80和443以外的端口上的流量：“阻止对除80和443之外的所有端口的请求”。

端口80和443是唯一的端口：

对于启用了中国网络的区域内的中国境内的HTTP / HTTPS流量
对于Cloudflare应用程序能够代理
当CloudFlare的缓存可用

当然如果你有钱，可以自定义端口，基本所有TCP端口都可以
<img src="port.png" width="100%" alt="端口">

官网说明：https://support.cloudflare.com/hc/en-us/articles/200169156-Which-ports-will-Cloudflare-work-with-