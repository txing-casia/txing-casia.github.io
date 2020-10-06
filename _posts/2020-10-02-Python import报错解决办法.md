---
layout:     post
title:      "Python import报错解决办法"
subtitle:   ""
date:       2020-10-02
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Python
---

# Python import报错解决办法

## 问题描述

import torch报错：

> from torch._C import * ImportError: numpy.core.multiarray failed to import

## 问题分析

这一类报错基本有一下几个原因：

- 所安装的库版本号不对；
- 说安装的库依赖于另一些未安装的库；
- 相同库安装了多个版本，产生了冲突；
- 错误版本的库通过uninstall操作未卸载干净；

## 处理办法

知道了出错的可能原因，接下来只要慢慢排除就能确定准确的病因了。

- **Step 1：**

在anaconda中打开所用环境，尝试

```python
conda uninstall numpy
# Or
pip uninstall numpy
```

注意：有时候conda uninstall会提示你连带删除很多其他的库，如果提示了，尽量就不要删除。使用pip方式，只删除一个库。

- **Step 2：**

找到anaconda所用环境中各个库的安装文件夹site-packages（D:\Anaconda3\envs\opensim-rl\Lib\site-packages）

- **Step 3：**

找到相关库的文件夹（我这里是找numpy），如果发现存在相同库的拨不通版本的文件夹，就可能是出现了重复安装，相互冲突的问题。删除重复安装的库的相关文件夹，如未重复安装，也可以删除并重新安装；

- **Step 4：**

在anaconda中打开所用环境安装所需库

```python
conda install numpy
# Or
pip install numpy
```

如果需要安装指定的版本号（eg 1.14.5），可通过一下方式实现：

```python
conda install numpy==1.14.5
# Or
pip install numpy==1.14.5
```





装好之后一般就不会报错了。

今后其它的一些库的报错也可以采用类似的方式进行处理。



2020年10月02日

Txing

