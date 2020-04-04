---
title: 使用python脚本下载bilibili视频
categories:
  - [技巧]
  - [学习]
tags:
date: 2019-09-21 23:09:00
translate_title: download-bilibilibili-videos-using-python-scripts
keywords:
description:
top_img: /2019/09/21/download-bilibilibili-videos-using-python-scripts/Python-BILIBILI.png
comments:
cover: /2019/09/21/download-bilibilibili-videos-using-python-scripts/Python-BILIBILI.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
Github地址：https://github.com/Henryhaohao/Bilibili_video_download
这个下载很耗服务器资源小CPU|RAM就别用了用下面这个
[post cid="65" cover="https:\/\/www.xyz.blue\/usr\/uploads\/2019\/09\/3014905791.png"/]
<img src="Python-BILIBILI.png" width="100%" alt="Python-BILIBILI">
```
#下载静态文件
wget https://github.com/q3aql/aria2-static-builds/releases/download/v1.34.0/aria2-1.34.0-linux-gnu-64bit-build1.tar.bz2
#解压文件并进入文件夹
tar jxvf aria2-*.tar.bz2 && rm -rf aria2-*.tar.bz2 && cd aria2-*
#开始安装
make install
```
如果安装的时候提示-bash: make: command not found错误，需要先运行命令：
Debian/Ubuntu系统：apt install make -y
CentOS系统：yum install make -y

安装Python3
```
apt-get install python3-pip -y
```
Centos百度
安装ffmpeg
```
apt-get install ffmpeg
```
Centos百度
```
git clone https://github.com/Henryhaohao/Bilibili_video_download.git
```
进入安装
```
pip3 install -r requirements.txt
```
按照提示输入即可


----------
问题
[post cid="63" /]


  