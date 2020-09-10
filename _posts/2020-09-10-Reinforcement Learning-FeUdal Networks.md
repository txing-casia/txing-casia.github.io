---
layout:     post
title:      "Reinforcement Learning | FeUdal Networks (FuN)"
subtitle:   ""
date:       2020-09-10
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
    - HRL
---

# FeUdal Networks for Hierarchical Reinforcement learning

论文链接：https://arxiv.org/abs/1703.01161

## 背景

利用分层强化学习将一个复杂任务拆分为不同维度的问题被认为是一种更自然、更类人的模式。例如人在吃饭的时候，人在宏观的维度作出决策要吃哪个菜，然后身体处于低维的层次，调动肌肉产生运动，完成夹菜和吃菜的动作。在这一过程中，高维的我们没有直接考虑调用哪条肌肉，而是做什么事。这一点正是目前的RL所缺乏的能力。



## 主要工作

FuN是在feudal reinforcement learning基础上的延展性工作，利用Manger学习子状态空间，设定子目标，将任务进行拆分；然后Worker处理子任务，完成Manger设置的子目标。

其主体框架如下：

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20200910-1.png)

其中，Manger和Worker都是RNN，只不过处理问题的时间分辨率不一样。Worker的c步被看做Manger的一步。网络动力学关系如下：

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20200910-2.png)

$$x_t$$是图像输入，$$z_t$$是提取的特征，$$s_t$$是输入给Manger RNN的状态。$$h^M$$是Manger网络的内部状态。通过$$f^{Mrnn}$$获得子目标$$\hat{g}_t$$、归一化子目标$$g_t$$。

通过一个线性映射$$\phi$$将$$c$$步的目标映射到嵌入向量$$w_t$$中。对于Work RNN，输入是状态$$z_t$$和网络内部状态$$h^W$$，$$U_t$$是输出矩阵。最后通过SoftMax选择最能满足子目标$$g_t$$的$$U_t$$。



（文章的主体思路就是以上内容，后续部分主要讨论网络更新，reward设定等细节，我打算看了代码之后在做分析）





## 总结

文章种在多个游戏中进行了实验对比，均取得了良好的实验效果。但遗憾的是，几乎所有的复现都说性能达不到文章中的水平。可能是实验中还有一些优化并未在文章中说明。







