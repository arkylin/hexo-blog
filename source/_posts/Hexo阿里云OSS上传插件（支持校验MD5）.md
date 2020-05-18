---
title: Hexo阿里云OSS上传插件（支持校验MD5）
categories:
  - [Hexo]
  - [Aliyun]
  - [脚本]
  - [学习]
tags:
  - [Hexo]
  - [Aliyun]
  - [阿里云]
  - [脚本]
  - [学习]
translate_title: hexo-alibaba-cloud-oss-upload-plugin-supports-check-md5
top: true
date: 2020-05-15 23:18:02
keywords:
description:
top_img: /2020/05/15/hexo-alibaba-cloud-oss-upload-plugin-supports-check-md5/deployer.png
comments:
cover: /2020/05/15/hexo-alibaba-cloud-oss-upload-plugin-supports-check-md5/deployer.png
toc: false
toc_number:
copyright:
mathjax:
katex:
hide:
---
这个 Github 里面有 NPM 里面也有，觉得原版hexo-deployer-ali-oss每次都要上传全部文件就自己做了一个，有时间更新

做这个之前属于完全没学习过nodejs，完全面向浏览器编程。该项目制作修改时间2020-05-15 19:45~2020-05-15 22:53

名称叫这个

hexo-deployer-aliyun-oss

预览

<img src="deployer1.png" width="100%" alt="Hexo阿里云OSS上传插件">
<img src="deployer.png" width="100%" alt="Hexo阿里云OSS上传插件">

#  hexo-deployer-aliyun-oss部署器使用说明

源项目：https://github.com/wertycn/hexo-deployer-ali-oss
该项目：https://github.com/arkylin/hexo-deployer-aliyun-oss

本项目在源项目的基础上增加了MD5校验工具，这样就可以避免上传重复文件耗时了，本地计算MD5的时间如果不是什么大文件，基本可以忽略不计，后续也可能会加入其他功能。例如：文件排除项、时间或文件大小比对等

在hexo项目下执行安装命令：

```
    npm install hexo-deployer-aliyun-oss --save
```

在hexo项目配置文件`_config.yml`中添加如下配置：

```
deploy:
  type: ali-oss
  region: <您的oss 区域代码>
  accessKeyId: <您的oss  accessKeyId>
  accessKeySecret: <您的oss accessKeySecret>
  bucket: <您的bucket name>
  
```

就这么简单 然后执行部署命令：

```
hexo g
```

```
hexo d
```

即可将项目部署到oss中 ，默认情况下，将文件上传到bucket的根目录下，如果需要部署到其他目录，请在deploy下添加remotePath选项进行指定

```
	remotePath:<您要部署的目录>
```