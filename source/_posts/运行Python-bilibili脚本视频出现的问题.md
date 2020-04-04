---
title: 运行Python-bilibili脚本视频出现的问题
categories:
  - [技巧]
tags:
date: 2019-09-21 23:12:00
translate_title: problems-with-running-pythonbilibilibili-script-videos
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
[post cid="62" /]
Imageio: 'ffmpeg-linux64-v3.3.1' was not found on your computer; downloading it now.

在使用Python的moviepy模块的时候，报出以下错误：
```
Imageio: 'ffmpeg.linux64' was not found on your computer; downloading it now.
Error while fetching file: <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:726)>.
Error while fetching file: <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:726)>.
Error while fetching file: <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:726)>.
Error while fetching file: <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:726)>.
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/me/anaconda2/lib/python3.7/site-packages/imageio/plugins/ffmpeg.py", line 73, in download
    force_download=force_download)
  File "/home/me/anaconda2/lib/python3.7/site-packages/imageio/core/fetching.py", line 127, in get_remote_file
    _fetch_file(url, filename)
  File "/home/me/anaconda2/lib/python3.7/site-packages/imageio/core/fetching.py", line 183, in _fetch_file
    os.path.basename(file_name))
IOError: Unable to download 'ffmpeg.linux64'. Perhaps there is a no internet connection? If there is, please report this problem.
```
错误原因：
imageio模块下载ffmpeg的时候，无法连接网络。
下载的路径可以参看:
./site-packages/imageio/core/fetching.py:63
./site-packages/imageio/plugins/ffmpeg.py:70
```
wget https://github.com/imageio/imageio-binaries/raw/master/ffmpeg/ffmpeg.linux64
```
```
mv ffmpeg.linux64 ~/.imageio/ffmpeg/
```
