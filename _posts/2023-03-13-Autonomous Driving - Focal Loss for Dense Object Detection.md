---
layout:     post
title:      "Autonomous Driving | Focal Loss for Dense Object Detection"
subtitle:   ""
date:       2023-03-13
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
---

## Focal Loss for Dense Object Detection

-  提出一种处理不平衡类别的损失函数，对分类准确的样本小权重，给不确定的样本大权重。在保持检测速度的情况下（一阶段检测器），精确度达到二阶段检测器水平
- 代码：https://github.com/facebookresearch/Detectron

### 1 Focal Loss

focal loss针对one-stage object detection场景中的类别不平衡问题设计（e.g., 1:1000）。首先，从交叉熵损失开始：
$$
CE(p,y)=\begin{cases}
-\log(p) ,&if \ y = 1\\ 
-\log(1-p) ,&otherwise
\end{cases}
$$
其中，$$y \in \{\pm 1\}$$表示真值类别，$$p\in[0,1]$$表示模型输出的属于类别$$y=1$$的类别概率。定义：
$$
p_t=\begin{cases}
p ,&if \ y = 1\\ 
1-p ,&otherwise
\end{cases}
$$
重写$$CE(p,y)=CE(p_t)=-\log(p_t)$$

#### 1.1 Balanced Cross Entropy

下调容易分类的样本权重，让模型关注困难的情况，在交叉熵损失函数基础上使用调制因子$$(1-p_t)^{\gamma}$$，其中$$\gamma\geq 0$$是可调参数，定义focal loss为：
$$
FL(p_t)=-(1-p_t)^{\gamma}\log(p_t)
$$








- ![Planning, acting and training with a learned model](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20221020-11.jpg)






### 总结



​	
