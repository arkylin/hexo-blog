---
title: NextCloud上传失败或删除失败解决方法
categories:
  - [技巧]
tags:
date: 2019-09-21 23:34:00
translate_title: nextcloud-upload-failure-or-delete-failure-solution
keywords:
description:
top_img: /2019/09/21/nextcloud-upload-failure-or-delete-failure-solution/Nextcloud-show.png
comments:
cover: /2019/09/21/nextcloud-upload-failure-or-delete-failure-solution/Nextcloud-show.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
```
#!/bin/bash
php /你的网站目录/occ maintenance:mode --on

mysql -u数据库账号 -p你的数据库密码 -e "
use 数据库名称;
DELETE FROM oc_file_locks WHERE 1;
exit"

php /你的网站目录/occ maintenance:mode --off
```
将以上保存为.sh脚本
在你的网站目录下面新建.php文件
```
<?php
        passthru("bash 上面你的脚本名称.sh");
?>
```
那么访问这个php文件就会运行之前的那个脚本
会比每次在SSH中输入会简便很多运行程序的目录是你的网站目录，如需修改请使用绝对路径

注意：在php.ini中删除禁用函数passthru()



----------
在NextCloud中添加应用--那个自定义链接的
<img src="Nextcloud-show.png" width="100%" alt="Nextcloud-show">


  