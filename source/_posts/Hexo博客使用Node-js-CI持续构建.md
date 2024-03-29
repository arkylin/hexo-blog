---
title: Hexo博客使用Node.js-CI持续构建
categories:
  - []
tags:
  - []
translate_title: hexo-blog-is-continuously-built-using-nodejsci
cover:
date: 2020-07-07 19:11:38
keywords:
description:
top_img:
comments:
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
本来打算使用Coding的持续集成的，但是他的限制好多，所以就用Github Actions

使用腾讯云储存COS+CDN并配合Coding的页面共同使用

```yml run.yml
# This workflow will do a clean install of node dependencies, build the source code and run tests across different versions of node
# For more information see: https://help.github.com/actions/language-and-framework-guides/using-nodejs-with-github-actions

name: Node.js CI

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [12.x]

    steps:
    - uses: actions/checkout@v2
    
    - name: 配置 Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}

    - name: 设置初始环境
      run: |
        sudo apt-get install -y python3 python3-pip
        sudo pip3 install setuptools
        sudo pip3 install coscmd
    - name: 安装扩展
      run: |
        npm install
    - name: 生成静态页面
      run: |
        node_modules/.bin/hexo g
    - name: 设置储存桶
      run: |
        COS_SECRET_ID='${{ secrets.COS_SECRET_ID }}'
        COS_SECRET_KEY='${{ secrets.COS_SECRET_KEY }}'
        COS_BUCKET_NAME='${{ secrets.COS_BUCKET_NAME }}'
        COS_BUCKET_REGION='${{ secrets.COS_BUCKET_REGION }}'
        coscmd config -a ${COS_SECRET_ID} -s ${COS_SECRET_KEY} -b ${COS_BUCKET_NAME} -r ${COS_BUCKET_REGION}
    - name: 上传文件
      run: |
        coscmd upload -rs ./public/ ./
    - name: 删除密钥文件
      run: |
        if [ -e "~/.cos.conf" ]; then
          sudo rm -rf ~/.cos.conf
          echo "文件已删除"
        else
          echo "文件已消失"
        fi
    - name: 配置Coding
      run: |
        CODING_PRIVATE_KEY='${{ secrets.CODING_PRIVATE_KEY }}'
        if [ ! -e "~/.ssh" ]; then
          mkdir ~/.ssh
        else
          sudo rm -rf ~/.ssh
          mkdir ~/.ssh
        fi
        echo ${CODING_PRIVATE_KEY} | sed 's/;/\n/g' > ~/.ssh/id_rsa
        sudo chmod -R 600 ~/.ssh/id_rsa
        ssh -o "StrictHostKeyChecking no" -T git@e.coding.net
        git config --global user.name "Arkylin"
        git config --global user.email "x@xyz.blue"
    - name: 部署文件到Coding
      run: |
        node_modules/.bin/hexo d
    - name: 删除Coding配置文件
      run: |
        git config --global --unset user.name
        git config --global --unset user.email
        if [ -e "~/.ssh" ]; then
          sudo rm -rf ~/.ssh
          echo "文件已删除"
        else
          echo "文件已消失"
        fi
```