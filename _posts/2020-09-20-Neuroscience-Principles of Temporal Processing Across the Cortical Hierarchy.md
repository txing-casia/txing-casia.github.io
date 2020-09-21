---
layout:     post
title:      "Neuroscience | Principles of Temporal Processing Across the Cortical Hierarchy"
subtitle:   ""
date:       2020-09-20
author:     "Txing" 
header-img: "img/post-bg-py.jpg"
tags:
    - Neuroscience
---

# Principles of Temporal Processing Across the Cortical Hierarchy  

论文链接：https://www.sciencedirect.com/science/article/pii/S0306452218302951?via%3Dihub

## 背景

 生活中存在着许多的时间和空间的尺度。对于空间结构的表现，在网络中通常是以Pooling，normalization，以及pattern completion的形式呈现。这些操作可以改变空间上的尺度，形成对空间鲁棒的识别能力。这篇文章回顾了一些列的时间上操作的研究，帮助理解temporal polling，temporal normalization，以及temporal  pattern completion。

- Pooling：建立高维的表征，形成对输入的鲁棒的缩放和转换
- normalization：增强特征选择以及输入的鲁棒性
- pattern completion：降低噪声和缺失数据的影响

如果空间上的这些操作能够帮助开展高效的不同尺度的分析，那么类似的，时间上的操作应该也能帮助对时间的结构分析。

## 主要工作

- 分层的大脑结构是通过语言、听觉、视觉任务来发现的。


- 三个问题：
  - 如何在功能层面定义时间操作；
  - 操作在皮层中实施的神经基础是什么；
  - 该操作被迭代的运用在多个皮层层级上吗；

### 时间上的信息处理原则

#### 1.1 Pooling in space and time

- pooling可以看做是一个"sum" or "max" operation，产生high-level representations  

- 方法：
  - 定义窗口宽度$$\tau$$
  - 在$$[t-\tau,t]$$的时间内发生了激活，则输出激活信号。
- 允许了精确的输入时间

- 生物证据：神经元的兴奋通过突出调节，在数秒至数小时内累积并持续（Kukushkin NV, Carew TJ (2017) Memory takes time. Neuron 95(2):259–279）

#### 1.2 Normalization in space and time  

- 神经系统需要非线性操作来增加刺激的选择性响应和一直过度激活（A feedforward architecture accounts for rapid categorization. Proc Natl Acad Sci 104 (15):6424–642）
- 空间Normalization是根据其他具有相似选择性的神经元收到的输入重新调整其响应的过程

$$
R_j=\gamma \bigg(\frac{D^n_i}{\sigma^n+\sum_k D^n_k}\bigg)
$$
神经元$$j$$的响应 $$R_j$$ ，$$D_i$$ 是输入信号$$\gamma$$是乘法尺度系数，$$\sigma$$是形状系数，$$n$$决定了非线性的程度。这个公式是“winner-take-all” mechanism: all neuralu units will be normalized to zero, except the unit thatr eceives the largest input drive.  

- Normalization in time  

- **neuronal adaptation**: the tendency of a neuron to decrease its response to a stimulus, when that stimulus was previously presented within some time window.
- temporal normalization can improve **neuronal sensitivity to sequential patterns**, and make them robust to changes in input gain  .
- **pop-out effect**: infrequently presented stimuli are less adapted and therefore produce larger responses than frequent stimuli.  

#### 1.3 Pattern completion in space and time  



## 总结

这篇文章讲得不够具体，temporal  pool 和 temporal  normalization 还行，似乎能和计算模型结合，可是 temporal  normalization 有什么好处呢？



