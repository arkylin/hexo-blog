---
title: Typecho在使用CDN的情况下获取真实IP
categories:
  - [技巧]
tags:
date: 2019-08-30 22:19:00
translate_title: typecho-gets-real-ip-using-cdn
keywords:
description:
top_img: /2019/08/30/typecho-gets-real-ip-using-cdn/CDN-con.png
comments:
cover: /2019/08/30/typecho-gets-real-ip-using-cdn/CDN-con.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
当我的文章或页面被评论时，网站后台会看到他们发评论的IP地址
<img src="CDN-con.png" width="100%" alt="CDN-con">
<img src="CDN-IP.png" width="100%" alt="CDN-IP">
那么如何在使用CDN的情况下获取用户的真实IP呢？
其实很简单
在```config.inc.php```末尾添加即可
```
if(isset($_SERVER['HTTP_X_FORWARDED_FOR'])){
    $list = explode(',',$_SERVER['HTTP_X_FORWARDED_FOR']);
    $_SERVER['REMOTE_ADDR'] = $list[0];
}
```
<img src="CDN-config.png" width="100%" alt="CDN-config">


----------
效果
<img src="CDN-new-con.png" width="100%" alt="CDN-new-con">
<img src="CDN-new-IP.png" width="100%" alt="CDN-new-IP">


  
  
  
  
  