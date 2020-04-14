---
title: '"/usr/share/cracklib/pw_dict.pwd: No such file or directory"解决方法'
categories:
  - [技巧]
  - [解决方案]
tags:
  - [密码]
date: 2020-02-01 15:52:00
translate_title: usr-share-cracklib-pwdictpwd-no-such-file-or-directory-solution
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
在Linux下修改用户密码



1.直接运行  passwd 用户名,输入新密码，再确认一遍，重启生效



2.如果出现以下错误：

```

/usr/share/cracklib/pw_dict.pwd: No such file or directory

```



原因是缺少cracklib库



解决办法：安装cracklib-dicts



Fedora 

```

dnf -y install cracklib-dicts

```