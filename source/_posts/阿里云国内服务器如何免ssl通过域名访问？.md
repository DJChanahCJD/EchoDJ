---
title: 阿里云国内服务器如何免SSL通过域名访问？
date: 2025-03-06 12:37:35
tags:
  - Cloudfare
comments: true
---


### 1. 在Cloudflare中添加域名![image-20250306125308007](https://djchan-xyz.pages.dev/file/AgACAgUAAyEGAASJIjr1AAICi2fJKkNiRFmcAj0XF-hbmD1J4iPBAAK7wDEbASNQVnQI1CCjrT_yAQADAgADeQADNgQ.png)

### 2. 在DNS设置中将域名解析（类型A）到服务器公网IP地址

![alt text](https://djchan-xyz.pages.dev/file/AgACAgUAAyEGAASJIjr1AAICjGfJKkNX6HpVguFlkrimpbTa6-ITAAK8wDEbASNQVjUupuoNQ5v-AQADAgADeQADNgQ.png)

### 3. 在页面规则中添加重写规则，转发URL到服务器端口

![alt text](https://djchan-xyz.pages.dev/file/AgACAgUAAyEGAASJIjr1AAICjWfJKkX-5p65qsPYG8IY6HIf-v1IAAK9wDEbASNQVh3CcVJsInPSAQADAgADeQADNgQ.png)



域名解析那里可以用子域名， `页面规则`cloudfare免费提供3个
因此理论上一个域名可以支持3个服务器

> （`规则/概述`那里虽然也能重定向，但是定向后可能多一个'/'，会导致404，如： http://your-server-ip//）