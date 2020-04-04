---
title: Whcms开启SSL提示重定向次数过多[ERR_TOO_MANY_REDIRECTS]
categories:
  - [技巧]
tags:
date: 2019-12-07 23:22:00
translate_title: too-many-times-for-whcms-to-enable-ssl-prompt-redirection
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
问题描述：

环境：Linux+Nginx

WHMCS系统配置SSL证书，安装都正常，并且https也可以访问。然后做了301跳转，将原来的域名http转到https上，均正常。 

之后到后台设置网站连接为https://xxx.com，网站直接挂掉。提示错误。

Chrome提示： ERR_TOO_MANY_REDIRECTS

IE提示错误: INET_E_REDIRECT_FAILED

问题解决：

多方排查，包括重装whmcs，均无法解决。联系官方也无法给出更多的答案。唯独可以通过https访问网站，但是无法在后台设置连接为https。这样带来的问题是https可以访问，但是用户无法注册和验证Email连接等操作。

最终解决方案为：在configuration.php的文件中，加入一条： 
```
$_SERVER['HTTPS'] = 'On';
```
然后重启Nginx。在后台设置https后，问题得到解决。