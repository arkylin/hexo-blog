---
title: 使用docker搭建邮件服务器
categories:
  - [常用配置]
  - [技巧]
tags:
date: 2019-08-10 17:21:00
translate_title: using-docker-to-build-mail-server
keywords:
description:
top_img: /2019/08/10/using-docker-to-build-mail-server/poste.png
comments:
cover: /2019/08/10/using-docker-to-build-mail-server/poste.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
使用```Poste.io```搭建
<img src="poste.png" width="100%" alt="poste">
```
docker run -d \
    --net=host \
    -v /etc/localtime:/etc/localtime:ro \
    -v /poste/data:/data \
    --name "mailserver" \
	--privileged=true \
	--restart=always \
    -h "mail.xyz.blue" \
    -t analogic/poste.io
```
具体见官网：https://poste.io


  