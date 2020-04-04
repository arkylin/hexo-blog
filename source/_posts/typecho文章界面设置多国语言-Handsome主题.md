---
title: typecho文章界面设置多国语言-Handsome主题
categories:
  - [技巧]
tags:
date: 2019-08-19 17:36:00
translate_title: typeecho-articles-interface-setting-up-multilingual-languages
keywords:
description:
top_img: /2019/08/19/typeecho-articles-interface-setting-up-multilingual-languages/changlang.png
comments:
cover: /2019/08/19/typeecho-articles-interface-setting-up-multilingual-languages/changlang.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
**手动切换英文需要在浏览器语言中把英文挪到最前**
<img src="changlang.png" width="100%" alt="changlang">


----------


此文章内容自动生成
主要适配handsome，其他主题稍微修改


----------
[collapse status="false" title="0.0.1"]
无
[/collapse]
[collapse status="false" title="0.0.2"]
替换原有```@```为不易输入的字符```&```
为避免输入麻烦将原有&zh_CN&改为\&CN\&
[/collapse]
[collapse status="false" title="0.0.3"]
使首页标题，浏览器标题支持多语言
[/collapse]
[collapse status="false" title="0.0.4"]
更新代码
```
if(!empty($_SERVER['HTTP_ACCEPT_LANGUAGE'])){
                    $lang = substr($_SERVER['HTTP_ACCEPT_LANGUAGE'],0,4);
                    if(preg_match("/zh-c/i",$lang)){
                        $langConfig = "CN";
                    }
                    elseif(preg_match("/en/i",$lang)){
                        $langConfig = "US";
                    }
                    else{
                        $langConfig = "US";
                    }
                }
                else{
                    $langConfig = 'US';
                }
```
**0.0.4修改只取前4位，这样只判断最优先的语言。如果取前5位，可能出现en,zh的情况，影响判断。**
[/collapse]
[collapse status="false" title="0.0.5"]
使侧栏热门文章、随机文章支持多语言
前<img src="change-old.png" width="100%" alt="change-old">
后<img src="change-new.png" width="100%" alt="change-new">
**方法**
在/libs/Content.php中添加
<img src="Content.png" width="100%" alt="Content">
随机文章同理
[/collapse]
[collapse status="false" title="0.0.6"]
使文章内引用文章支持多语言
**方法**
在/libs/Content.php中添加
更改quoteOtherPostCallback函数
<img src="Content-006.png" width="100%" alt="Content-006">
[/collapse]
[collapse status="false" title="0.0.7"]
使归档支持多语言
**方法**
在/files.php中更改
大约55行的位置
<img src="files-007.png" width="100%" alt="files-007">
效果
<img src="arv-007.png" width="100%" alt="arv-007">
[/collapse]
----------


**0.0.3教程开始**
在主题的functions.php末尾增加两段函数
```
function getLang($content, $lang){
    preg_match_all('%&'.$lang.'.*?&(.*?)&/'.$lang.'&%si',$content,$match);
    return(str_replace("&/$lang&" , "",str_replace("&$lang&" , "",str_replace("&$lang&<br>" ,"" ,$match[0][0]))));
}
```
以上函数可自定义修改
如```$pa = '%@'.$lang.'.*?@(.*?)@/'.$lang.'@%si';```中的```@(.*?)@```。
```(.*?)```匹配任意长度的任意字符非贪婪模式遇到```/```字符停止
```
function _lan($content){
    if(!empty($_SERVER['HTTP_ACCEPT_LANGUAGE'])){
        $lang = $_SERVER['HTTP_ACCEPT_LANGUAGE'];
        if(preg_match("/zh-cn/i",$lang)){ #此处见0.0.4更新
            $langConfig = "CN";
        }
        else{
            $langConfig = "US";#此处见0.0.4更新
        }
    }
    else{
        $langConfig = 'US';
    }
    print(getLang($content, $langConfig));
}
```
在post.php中找到
```
<?php echo Content::postContent($this,$this->user->hasLogin());?>
```
改为
```
<?php echo _lan(Content::postContent($this,$this->user->hasLogin()));?>
```
在libs目录下找到Content.php
<img src="Content-003tech.png" width="100%" alt="Content-003tech">
这个是为了更改摘要显示
更改为以下
```
if(!empty($_SERVER['HTTP_ACCEPT_LANGUAGE'])){
    $lang = $_SERVER['HTTP_ACCEPT_LANGUAGE'];
        if(preg_match("/zh-cn/i",$lang)){#此处见0.0.4更新
            $langConfig = "CN";
            }
        else{
            $langConfig = "US";#此处见0.0.4更新
            }
    }
    else{
        $langConfig = 'US';
        }
        $content = getLang($content, $langConfig);
```
由于```$langConfig```为局部变量，所以就把程序复制了一份

***标题设置多国语言***
在主题functions.php中添加两段函数
```
function getLanPi($content, $lang){
    preg_match_all($lang,$content,$match);
    if($lang == "CN"){
        preg_match_all ('/`.*`/', $content,$match);
        return(str_replace("`" , "",$match[0][0]));
    }
    else{
        preg_match_all ('/~.*~/', $content,$match);
        return(str_replace("~" , "",$match[0][0]));
    }
    
}
```
```
function _lanPi($content){
    if(!empty($_SERVER['HTTP_ACCEPT_LANGUAGE'])){
        $lang = $_SERVER['HTTP_ACCEPT_LANGUAGE'];
        if(preg_match("/zh-cn/i",$lang)){#此处见0.0.4更新
            $langConfig = "CN";
            #$langConfig = "en_US";
        }
        else{
            $langConfig = "US";#此处见0.0.4更新
        }
    }
    else{
        $langConfig = 'US';
    }
    return(getLanPi($content, $langConfig));
}
```
中文用\`标题\`,英文用~标题~隔开
然后
在/var/Widget/Archive.php中添加如图
<img src="arv-003tech.png" width="100%" alt="arv-003tech">
以修改archiveTitle()
然后
在主题pages.php中修改文章页面标题
如图
<img src="pages-003tech.png" width="100%" alt="pages-003tech">
然后
在主题libs/Content.php修改echoPostList()来修改首页标题
如图
<img src="Content-echo-003tech.png" width="100%" alt="Content-echo-003tech">


实现方法
```
&CN&
你好，世界！
&/CN&
&US&
Hello,World!
&/US&

标题
\`中文\`
~英文~
```

后台界面，（如果看不下去，以后可能会修改）。
<img src="change-lang-admin-003.png" width="100%" alt="change-lang-admin-003">


----------


**然后问题就是针对AMP/MIP的插件还需要修改**
**Handsome阅读模式出现全部内容显示，还需修改**
~~Handsome引用文章页面出现全部内容显示，还需修改~~ 已解决见0.0.6
**以后会配置typecho后台文章界面显示单语言**
~~侧栏热门文章出现全标题，还需修改~~ 已解决 见0.0.5
**以后会添加其他功能如可以手动切换语言等**
~~文章归档页面出现全标题，还需修改~~ 已解决 见0.0.7


----------
!!!
[scode type="blue"]
版权声明：本文为原创文章，版权归 KYLIN 所有，转载请注明出处！</br>
本文链接：<font color="#0000FF">https://www.xyz.blue/archives/typeecho-articles-interface-setting-up-multilingual-languages.html</font></br>
如教程需要更新，或者相关链接出现404，可以在文章下面评论留言。
[/scode]
!!!


  
  
  
  
  
  
  
  
  
  
  
  