---
title: Typecho迁移Hexo的Python脚本
categories:
  - [技巧]
  - [脚本]
  - [解决方案]
tags:
  - [Typecho]
	- [技巧]
  - [脚本]
  - [解决方案]
translate_title: typecho-migrates-hexo-python-script
top_img: /2020/04/09/typecho-migrates-hexo-python-script/main.png
cover: /2020/04/09/typecho-migrates-hexo-python-script/main.png
date: 2020-04-09 10:59:42
keywords:
description:
comments:
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
最近换博客Typecho感觉还是有点臃肿。。好吧 虽然相比WordPress好多了
所以就换了Hexo，而且在本地也支持方便迁移，哈
脚本如下，按照自己的修改
```Python
import requests
import os
import shutil
import re
from subprocess import Popen, PIPE 
from bs4 import BeautifulSoup

for i in range(0,123):
    url='https://your.domain/admin/write-post.php?cid='
    url=url+str(i)

    cookies_text=r'你的cookies'

    cookies = {}
    for line in cookies_text.split(';'):
        key, value = line.split('=', 1)
        cookies[key] = value

    resp = requests.get(url,cookies=cookies)
    #<Response [404]>
    #<Response [200]>
    if str(resp) == '<Response [200]>':
        get_text=resp.text
        html=BeautifulSoup(get_text,'html.parser')
        get_title=html.find('input',id='title')["value"]
        get_title_translate=html.find('input',id='slug')["value"]
        get_data=html.find('textarea',id='text').text
        get_time=html.find('input',id='date')["value"]+":00"        

        check_1=html.find('input',id='category-1').attrs#常用配置
        check_2=html.find('input',id='category-2').attrs#技巧
        check_3=html.find('input',id='category-3').attrs#生活随笔
        check_4=html.find('input',id='category-4').attrs#学习
        check_10=html.find('input',id='category-10').attrs#Android Develop

        #替换标题中文件夹禁用字符
        get_title_replace=get_title
        get_title_replace=get_title_replace.replace("\\","")
        get_title_replace=get_title_replace.replace("/","")
        get_title_replace=get_title_replace.replace(":","：")
        get_title_replace=get_title_replace.replace("*","")
        get_title_replace=get_title_replace.replace("?","？")
        get_title_replace=get_title_replace.replace("\"","")
        get_title_replace=get_title_replace.replace("<","《")
        get_title_replace=get_title_replace.replace(">","》")
        get_title_replace=get_title_replace.replace("|","")
        #Hexo规则
        get_title_replace=get_title_replace.replace(" ","-")

        if "checked" in check_1:
            check_1_status=check_1["checked"]
            check_1_name="常用配置"
        else:
            check_1_status="false"
            check_1_name=""

        if "checked" in check_2:
            check_2_status=check_2["checked"]
            check_2_name="技巧"
        else:
            check_2_status="false"
            check_2_name=""

        if "checked" in check_3:
            check_3_status=check_3["checked"]
            check_3_name="生活随笔"
        else:
            check_3_status="false"
            check_3_name=""

        if "checked" in check_4:
            check_4_status=check_4["checked"]
            check_4_name="学习"
        else:
            check_4_status="false"
            check_4_name=""

        if "checked" in check_10:
            check_10_status=check_10["checked"]
            check_10_name="Android Develop"
        else:
            check_10_status="false"
            check_10_name=""

        #写入hexo
        os.chdir('C:\/Users\/Kylin\/Desktop\/Blog\/source\/_posts')
        #os_command_origin="powershell -command "
        #poweshell部分文字输入不进去好像有“”等
        os_command_origin="cmd /c "
        hexo_new_posts='hexo new '+get_title_replace
        #os.system(os_command_origin+hexo_new_posts)
        #print(os_command_origin+hexo_new_posts)
        #changed_title=os.popen(os_command_origin+hexo_new_posts).read().decode('utf-8')
        changed_title=Popen(os_command_origin+hexo_new_posts, shell=True,stdout=PIPE, stderr=PIPE).stdout.read().decode('utf-8')
        get_true_title_re=re.compile(r'INFO  Created: ~\\Desktop\\Blog\\source\\_posts\\(.*?)\.md')
        true_title=get_true_title_re.findall(changed_title)[0]

        #图片标签
        #re_img=u'!\[(.*?)\]\[([1-9]*)\]'
        #re_img_get_name=re.search(re_img,get_data)[1]
        #re_img_get_num=re.search(re_img,get_data)[2]
        #img_src_stick='<img src=\"'+'\" width="100%" alt=\"'+'\">'

        #查找写入配置
        #搜寻图片
        get_img_name_re=re.compile(r'!\[(.*?)\]\[[0-9]*\]')
        get_img_url_name_re=re.compile(r'\[[0-9]*\]: https://www\.xyz\.blue/usr/uploads/[0-9]*/[0-9]*/([0-9]*?\.png)')
        get_img_name_lists=get_img_name_re.findall(get_data)
        get_img_url_lists=get_img_url_name_re.findall(get_data)
        #替换图片文本
        get_data=re.sub(get_img_url_name_re,"",get_data)
        if len(get_img_name_lists) == len(get_img_url_lists) and get_img_name_lists != []:
            for j in range(0,len(get_img_name_lists)):
                img_src="<img src=\""+get_img_name_lists[j]+"\" width=\"100%\" alt=\""+get_img_name_lists[j].replace(".png","")+"\">"
                get_data=get_data.replace("!["+get_img_name_lists[j]+"]"+"["+str(j+1)+"]",img_src)#![poste.png][1]
                #移动图片
                if not os.path.isfile(true_title+'/'+get_img_name_lists[j]):
                    shutil.move('uploads/'+get_img_url_lists[j],true_title+'/'+get_img_name_lists[j])
                    print(true_title+'/'+get_img_name_lists[j])
        else:
            print("图片匹配出错")

        if get_img_name_lists != []:
            get_top_img='/'+get_time[0:4]+'/'+get_time[5:7]+'/'+get_time[8:10]+'/'+get_title_translate+'/'+get_img_name_lists[0]
        else:
            get_top_img=''
            print("cid:",i,"未找到图片")

        #合成写入内容
        ha_temp_main=['---\n']
        ha_temp_main.append('title: '+get_title+'\n')#'title: 使用docker搭建邮件服务器\n'
        #设置分类
        ha_temp_main.append('categories:\n')
        if check_1_status == "true":
            ha_temp_main.append('  - ['+check_1_name+']\n')#'  - []\n'
        if check_2_status == "true":
            ha_temp_main.append('  - ['+check_2_name+']\n')#'  - []\n'
        if check_3_status == "true":
            ha_temp_main.append('  - ['+check_3_name+']\n')#'  - []\n'
        if check_4_status == "true":
            ha_temp_main.append('  - ['+check_4_name+']\n')#'  - []\n'
        if check_10_status == "true":
            ha_temp_main.append('  - ['+check_10_name+']\n')#'  - []\n'
        #标签不知道怎么加，而且之前也没设置过标签，以后自己慢慢加吧
        ha_temp_main.append('tags:\n')
        ha_temp_main.append('')#'  - []\n'
        ha_temp_main.append('date: '+get_time+'\n')#date: 2020-04-02 23:12:09\n
        ha_temp_main.append('translate_title: '+get_title_translate+'\n')#'translate_title:\n'
        #翻译不同会让头图404
        #ha_temp_main.append('translate_title:\n')#'translate_title:\n'
        ha_temp_t_1=['keywords:\n', 'description:\n',]
        ha_temp_main.append(ha_temp_t_1)
        ha_temp_main.append('top_img: '+get_top_img+'\n')#'top_img:\n'
        ha_temp_t_2=['toc:\n', 'toc_number:\n', 'copyright:\n', 'mathjax:\n', 'katex:\n', 'hide:\n', 'top:\n', '---\n']
        ha_temp_main.append('comments:\n')
        ha_temp_main.append('cover: '+get_top_img+'\n')#'cover:\n'
        ha_temp_main.append(ha_temp_t_2)
        ha_temp_main.append(get_data)
        #写入文件
        f=open(true_title+".md","w",encoding='utf-8')
        for lines in ha_temp_main:
            f.writelines(lines)
        f.close()
        print('转移cid:',i,"成功")

    else:
        print('cid:',i,"找不到")
```
<img src="main.png" width="100%" alt="main.py">
代码文件：<a href="main.py" download="main.py">点击下载</a>