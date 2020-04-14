---
title: 解决申请 HSTS 时 Warning： Unnecessary HSTS header over HTTP 的方法
categories:
  - [技巧]
tags:
  - [HSTS]
  - [HTTPS]
  - [443]
date: 2019-10-02 01:26:00
translate_title: warning-unnecessary-hsts-header-over-http-method-for-hsts-application
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
**Nginx**

修改配置文件为如下所示：

```

map $scheme $hsts_header {

    https   "max-age=31536000; includeSubDomains; preload";

    }

server {

......(others)

  add_header Strict-Transport-Security $hsts_header;

......(others)

}

```

修改后重启Nginx。



**Apache**

修改配置文件为如下所示：

```

<VirtualHost *:443>

......(others)

    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"

......(others)

</VirtualHost>

```

修改后重启Apache。