---
title: 搭建drawio
categories:
  - [技巧]
tags:
date: 2019-10-02 01:22:00
translate_title: build-drawio
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
搭建drawio
draw.io的github地址在https://github.com/jgraph/drawio，可以在release下载正式版本，我们此次下载的是源码版本。

```
git clone https://github.com/jgraph/drawio.git
```
编译的话，需要安装ant等环境。

```
yum install -y ant java-1.8.0-openjdk-devel
```
接下来，就是需要编译了。
```
cd etc/build && ant
```
如果需要生成war包的话，则如下编译
```
cd etc/build && ant war
```
编译成功后，war包会生成在drawio/build/draw.war，如果你有tomcat或java的服务环境，可以使用war包部署。整体来说，还是不用war的方便一些，只要有Apache或Nginx就可以搞定了。