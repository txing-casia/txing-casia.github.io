---
layout:     post
title:      "Windows通过SSH向Linux服务器上传文件"
subtitle:   ""
date:       2019-09-06
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Windows
    - Linux
---

# Windows通过SSH向Linux服务器上传文件

这里需要使用到PuTTY的pscp工具，因此需要在[PuTTY官网](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)下载并安装。

- **Step 1**: Install PuTTY.

- **Step 2**: Open cmd window, and **enter the path of your PuTTY files**. 

  ```
  d:
  cd ./Program Files (x86)/Putty
  ```

- **Step 3**: Use 'pscp' order, and wait for transmission.

  ```
  pscp D:\download\Anaconda3-2019.07-Linux-x86_64.sh Txing@***.**.**.**.**:/home/Txing/anaconda3/
  ```

  

