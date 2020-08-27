---
layout:     post
title:      "Pycharm连接Linux服务器配置方法"
subtitle:   "这种调用其实很鸡肋，尽可能不要跨软件调用，"
date:       2019-10-24
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Linux
    - Pycharm
---

# Pycharm连接Linux服务器配置方法

今天手贱在pycharm删除不用的解释器时把几个月前配置的远程解释器删了，查了下资料重新恢复，顺便留个笔记以防万一。

- Step 1：打开python工程，点击Tool->Deployment->Configration
- Step 2：填写远程解释器Name（随意命名），在connection页面选择类型为SFTP，填写host，user name，password等等。最后可以点击test SFTP connectioin测试一下。转到mapping页面，选择将windows下的代码和服务器上代码相连，填写本地Local path，服务器path，apply。
- Step 3：在菜单栏，File -> Settings… -> Project ×× -> Project Interpreter，点击Add添加解释器。选择SSH Interpreter。
- Step 4：填写服务器的 Host 地址，端口Port，用户名Username，填好后，下一步Next。
- Step 5：填写密码 Password，下一步Next。
- Step 6：选择服务器上Python解释器的位置，以及服务器上的远程同步文件夹Sync folders（可设置多个）。服务器上可以使用命令which python找到当前环境Python的安装位置。如果通过anaconda安装了新的python环境，可以用conda activate opensim-rl，切换默认的base环境到opensim-rl环境，再用which python查找位置。输入好路径，点击Finish，配置结束。

2019年10月24日

Txing