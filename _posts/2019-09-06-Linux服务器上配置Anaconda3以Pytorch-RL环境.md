---
layout:     post
title:      "Linux服务器上配置Anaconda3以Pytorch-RL环境"
subtitle:   ""
date:       2019-09-06
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Windows
    - Linux
    - Reinforcement Learning
---

# Linux服务器上配置Anaconda3以Pytorch-RL环境

通过上传安装包的方式在服务器安装Anaconda3只需要在**相应的目录下**执行一句命令：

```
bash Anaconda3-2019.07-Linux-x86_64.sh
```

安装途中会提示安装位置为默认路径：

```
/home/userNAME/anaconda3
```

等待安装完成后，切换到Anaconda所在目录，打开Anaconda创建新环境：

这里由于我之前进行的配置都是基于Python 3.6.1的，所以这里要求新环境的Python也为3.6.1，但是在安装过程中出现了路径失效的情况（可能老版本的下载通道少一些），新建失败。这里就只要求版本为3.6即可。输入以下指令，新建环境成功。

```
conda create --name opensim-rl python=3.6
source activate opensim-rl
```

环境建好后可以通过以下指令进行版本检查：

```
python --version
> Python 3.6.9 :: Anaconda, Inc.
```

接下来激活新建的环境并进行配置：

```
source activate opensim-rl
```

参考我之前的一篇经验（[Anaconda新环境下快速安装多个Python软件包](<https://zhuanlan.zhihu.com/p/30636305>)）可以快速在新环境中批量安装软件包：

**在新环境中**输入：

```
conda install anaconda 
```

等待安装完成。

这里就基本把会调用的包都装上了，接下来开始安装PyTorch。

---

继续呆在**激活的opensim-rl环境**里

由于PyTorch版本的选择和cuda版本有关，虽然因此先检查服务器上的cuda版本号：

```
cat /usr/local/cuda/version.txt
> CUDA Version 9.0.103
```

虽然PyTorch目前支持的最低版本是9.2，先选9.2，找到安装指令格式，再把9.2改成9.0（这个升级只有一两个月，自觉上应该是可以这样操作的），尝试安装

```
conda install pytorch torchvision cudatoolkit=9.0 -c pytorch
```

这里有一点需要注意：anaconda提供的pytorch的url国内不少很稳定，在下载的时候链接经常挂掉。

这里给出几个策略：

1. （玄学方法）在早晚下载低谷的时候下，速度会比较快。

2. **（科学方法1）开VPN下载，会很稳定。**
3. （科学方法2）添加国内清华、中科大的镜像源，多一些链接路径会增加成功的概率。

这里我们用了[Lantern](<http://www.landeng26.info/>)每个月500M的免费流量帮助下载，速度比较快

---

最后开始安装gym

```
pip install gym
```

搞定！





