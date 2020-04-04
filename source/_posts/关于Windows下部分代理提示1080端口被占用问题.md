---
title: 关于Windows下部分代理提示1080端口被占用问题
categories:
  - [技巧]
tags:
date: 2019-08-20 17:49:00
translate_title: about-the-occupancy-of-port-1080-under-windows-agent-tips
keywords:
description:
top_img: /2019/08/20/about-the-occupancy-of-port-1080-under-windows-agent-tips/ssr1080.png
comments:
cover: /2019/08/20/about-the-occupancy-of-port-1080-under-windows-agent-tips/ssr1080.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
拿比较热门的￥￥R举例
会提示：<img src="ssr1080.png" width="100%" alt="ssr1080">
***一***
```
netstat -aon|findstr "1080"
```
查看端口是否被占用

***二***
由于更新系统导致安装了某种安全更新[Windows Sockets Error Codes][2]
运行命令
```
netsh interface ipv4 show excludedportrange protocol=tcp
```
下面是我的电脑
![ssrport.png][3]
提示1031-1130被占用，那么方法就是使用里面未提及的端口，或者删除更新。


  
  [2]: https://docs.microsoft.com/en-us/windows/win32/winsock/windows-sockets-error-codes-2
  