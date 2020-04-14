---
title: Docker内安装Mysql/Mariadb提示"服务器和客户端上指示的 HTTPS 之间不匹配"解决方案
categories:
  - [技巧]
tags:
  - [Docker]
date: 2020-01-30 13:05:00
translate_title: when-installing-mysql-mariadb-in-docker-the-solution-of-mismatch-between-https-indicated-on-server-and-client-will-be-prompted
keywords:
description:
top_img: /2020/01/30/when-installing-mysql-mariadb-in-docker-the-solution-of-mismatch-between-https-indicated-on-server-and-client-will-be-prompted/phpmyadmin_ssl.png
comments:
cover: /2020/01/30/when-installing-mysql-mariadb-in-docker-the-solution-of-mismatch-between-https-indicated-on-server-and-client-will-be-prompted/phpmyadmin_ssl.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
使用docker安装phpmyadmin，启动成功登录，出现提示：服务器和客户端上指示的 HTTPS 之间不匹配。这可能导致 phpMyAdmin 无法正常工作或存在安全风险。请修复您的服务器配置以正确指示 HTTPS。



<img src="phpmyadmin_ssl.png" width="100%" alt="phpmyadmin_ssl">



以前phpmyadmin是直接安装在nginx里的，没有这个问题。现在用caddy做为代理。外部访问使用https，内部用http。



解决方法是修改参数：



$cfg[‘PmaAbsoluteUri’] = ‘https://pma.xxx.com’;



在docker中启动，可以在启动时指定环境参数（该参数在官方文档里没有但是有效）：



-e PMA_ABSOLUTE_URI=https://pma.xxx.com





  