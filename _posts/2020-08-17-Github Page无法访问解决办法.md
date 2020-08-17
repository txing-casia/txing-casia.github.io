---
layout:     post
title:      "Github Page无法访问解决办法"
subtitle:   ""
date:       2020-08-17
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Github Page
---

# Github Page无法访问解决办法

###问题描述：

在2020年5月份左右，我的Github Page打不开了，之前都是刻意正常访问的。



###解决思路：

- 刚开始以为是自己的新上传的修改有问题，做了回调，但是依然打不开。
- 尝试网上搜索类似情况，有人说是页面从http改为https了。但这个消息是2017年的，所以也不是原因。
- 以为是我申请的域名过期了，于是设置取消了自定义的域名，但依旧打不开。
- 以为是Github被墙了，但Github主页和其它仓库都能打开。
- 有人说是Github在国内功能做了阉割，去掉了Github Page功能。但是我能打开别的一些Github Page（不能保证都能打开）。
- 尝试翻墙打开我的Github Page，能够打开。而有些Github Page不翻墙也能打开。那么应该是我的网络有问题。
- 尝试修改host，没什么用。
- 看到有人说是电信的DNS的问题，尝试换成阿里的，成功解决问题。



### 具体步骤：

- Step1：修改路由器的自动配置DNS为手动；
- Step2：填入阿里的DNS：223.5.5.5和223.6.6.6；





Txing

2020年8月17日





























