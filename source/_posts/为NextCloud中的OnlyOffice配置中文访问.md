---
title: 为NextCloud中的OnlyOffice配置中文访问
categories:
  - [技巧]
tags:
date: 2019-09-18 21:24:00
translate_title: configure-chinese-access-for-onlyoffice-in-nextcloud
keywords:
description:
top_img: /2019/09/18/configure-chinese-access-for-onlyoffice-in-nextcloud/OnlyOffice-1.png
comments:
cover: /2019/09/18/configure-chinese-access-for-onlyoffice-in-nextcloud/OnlyOffice-1.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
感觉他那个自带的自动识别网站语言有问题
所以进行了修改
进入
<img src="OnlyOffice-1.png" width="100%" alt="OnlyOffice-1">
修改两项内容
下面这个是指新建文档时默认进入编辑页面的中英文
<img src="OnlyOffice-2.png" width="100%" alt="OnlyOffice-2">
下面这个是指编辑文档时默认进入编辑页面的中英文
<img src="OnlyOffice-3.png" width="100%" alt="OnlyOffice-3">
代码跟之前的一样
[post cid="37" /]
```
        if(!empty($_SERVER['HTTP_ACCEPT_LANGUAGE'])){
            $lang = substr($_SERVER['HTTP_ACCEPT_LANGUAGE'],0,4);
            if(preg_match("/zh-c/i",$lang)){
                $langConfig = "zh-CN";
            }
            elseif(preg_match("/en/i",$lang)){
                $langConfig = "en";
            }
            else{
                $langConfig = "en";
            }
        }
        else{
            $langConfig = 'en';
        }

        $lang = $langConfig;
```


  
  
  