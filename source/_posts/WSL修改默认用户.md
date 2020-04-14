---
title: WSL修改默认用户
categories:
  - [技巧]
tags:
  - [WSL]
  - [默认用户]
date: 2020-03-06 16:52:00
translate_title: wsl-modify-default-user
keywords:
description:
top_img: /2020/03/06/wsl-modify-default-user/WSL_help.png
comments:
cover: /2020/03/06/wsl-modify-default-user/WSL_help.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
以管理员的方式打开PowerShell或者cmd

①：如果你是在Microsoft Store里面安装的，则切换到`C:\Users\Kylin\AppData\Local\Microsoft\WindowsApps`目录下

②：如果你是在GitHub等其他位置通过源码安装的请切换到你的源码目录



注：一下fedoraremix可以更换为ubuntu、debian等已安装的发行版



输入下面这个查看帮助

```
./fedoraremix --help
```

<img src="WSL_help.png" width="100%" alt="WSL_help">

由于用Microsoft Store安装的已经预先设置好了环境变量所以可以不加`./`

输入下面这个修改默认用户

```
fedoraremix config  --default-user root
```

<img src="WSL_Change_User.png" width="100%" alt="WSL_Change_User">





  

  