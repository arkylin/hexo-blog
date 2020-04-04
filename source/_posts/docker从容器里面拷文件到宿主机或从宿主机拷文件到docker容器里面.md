---
title: docker从容器里面拷文件到宿主机或从宿主机拷文件到docker容器里面
categories:
  - [技巧]
tags:
date: 2019-10-02 15:39:00
translate_title: docker-copies-files-from-container-to-host-or-from-host-to-docker-container
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
1、从容器里面拷文件到宿主机？

假设容器名为testtomcat,要从容器里面拷贝的文件路为：/usr/local/tomcat/webapps/test/js/test.js,  现在要将test.js从容器里面拷到宿主机的/opt路径下面，那么命令应该怎么写呢？

```
docker cp testtomcat：/usr/local/tomcat/webapps/test/js/test.js /opt
```

2、从宿主机拷文件到容器里面

假设容器名为testtomcat,现在要将宿主机/opt/test.js文件拷贝到容器里面的/usr/local/tomcat/webapps/test/js路径下面，那么命令该怎么写呢？   

```
docker cp /opt/test.js testtomcat：/usr/local/tomcat/webapps/test/js
```