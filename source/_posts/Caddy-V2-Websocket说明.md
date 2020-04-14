---
title: Caddy_V2 Websocket说明
categories:
  - [技巧]
  - [学习]
tags:
  - [Caddy]
date: 2020-02-17 23:29:00
translate_title: caddy-v2-websocket-description
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
https://caddy.community/t/unrecognized-subdirective-websocket/6493



```

header_up Connection {http.request.header.Connection}

header_up Upgrade {http.request.header.Upgrade}

```



In v2, you do not need to do anything to enable websockets.

Websockets should work with v2’s reverse proxy by default.