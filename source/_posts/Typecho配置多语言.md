---
title: Typecho配置多语言
categories:
  - [技巧]
tags:
date: 2019-08-21 18:00:00
translate_title: typecho-configuration-multilingualism
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
[post cid="37" cover="https:\/\/www.xyz.blue\/usr\/uploads\/2019\/08\/1378905739.png"/]
更新一下，上次的那个方法太复杂。
增加en站点，就是重新搭建一个博客，数据库还是同一个不过数据库前缀可以修改
如果分开数据库的话对Sqlite不友好，Mysql随意
编辑config.inc.php
**禁止直接复制，具体内容需要修改为你自己的数据库**
Mysql的话对照你原来的配置修改，大体框架不变
```
/** 定义数据库参数 */
#$db = new Typecho_Db('Pdo_SQLite', '');
#$db->addServer(array (
#  'file' => '/home/wwwroot/www.xyz.blue/usr/.db',
#), Typecho_Db::READ | Typecho_Db::WRITE);
#Typecho_Db::set($db);
if(!empty($_SERVER['HTTP_ACCEPT_LANGUAGE'])){
  $lang = substr($_SERVER['HTTP_ACCEPT_LANGUAGE'],0,4);
  if(preg_match("/zh-c/i",$lang)){
      $db = new Typecho_Db('Pdo_SQLite', '');
      $db->addServer(array (
      'file' => '/home/wwwroot/www.xyz.blue/usr/.db',
      ), Typecho_Db::READ | Typecho_Db::WRITE);
      Typecho_Db::set($db);
  }
  elseif(preg_match("/en/i",$lang)){
      $db = new Typecho_Db('Pdo_SQLite', 'en_');
      $db->addServer(array (
      'file' => '/home/wwwroot/www.xyz.blue/usr/.db',
      ), Typecho_Db::READ | Typecho_Db::WRITE);
      Typecho_Db::set($db);
  }
  else{
      $db = new Typecho_Db('Pdo_SQLite', 'en_');
      $db->addServer(array (
      'file' => '/home/wwwroot/www.xyz.blue/usr/.db',
      ), Typecho_Db::READ | Typecho_Db::WRITE);
      Typecho_Db::set($db);
  }
}
else{
  $db = new Typecho_Db('Pdo_SQLite', 'en_');
      $db->addServer(array (
      'file' => '/home/wwwroot/www.xyz.blue/usr/.db',
      ), Typecho_Db::READ | Typecho_Db::WRITE);
      Typecho_Db::set($db);
}
```
上面这个就是以你访客的浏览器的第一语言为参考来调用不同数据库来实现多语言