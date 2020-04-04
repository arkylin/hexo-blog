---
title: 非交互式修改SSH密码
categories:
  - [技巧]
tags:
date: 2020-01-28 19:39:00
translate_title: non-interactive-ssh-password-modification
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
方式1：
```
echo "password" | passwd testuser --stdin > /dev/null 2>&1
```
方式2：
```
echo testuser:password|chpasswd
```

**在shell脚本和Dockerfile中请使用方式2**