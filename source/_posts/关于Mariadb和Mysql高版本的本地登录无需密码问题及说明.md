---
title: 关于Mariadb和Mysql高版本的本地登录无需密码问题及说明
categories:
  - [技巧]
tags:
  - [MariaDB]
  - [MySQL]
date: 2020-01-08 19:57:00
translate_title: no-password-problem-and-instructions-for-local-login-of-mariadb-and-mysql
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

MariaDB [mysql]> show grants for 'root'@'localhost';

```

输入命令

```

GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED VIA mysql_native_password USING '*那一坨东西' WITH GRANT OPTION;

```