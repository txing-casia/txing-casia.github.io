---
layout:     post
title:      "Neuroscience | Reservoir Computing Model of Prefrontal Cortex"
subtitle:   ""
date:       2020-09-28
author:     "Txing" 
header-img: "img/post-bg-py.jpg"
tags:
    - Neuroscience
---

# Reservoir computing model of prefrontal cortex creates novel combinations of previous navigation sequences from hippocampal place-cell replay with spatial reward propagation

论文链接：https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1006624

## 背景

小鼠在复杂空间导航的能力与海马体中位置细胞的回放有关。spatial navigation capacity involves the replay of hippocampal place-cells during awake states  

本文让小鼠学习TSP问题，学会了强化学习中的空间信用分配问题（spatial credit assignment）

这篇工作基于两篇之前的工作：

- Gupta AS, van der Meer MA, Touretzky DS, Redish AD. Hippocampal replay is not a simple function of experience. Neuron. 2010; 65(5):695–705. https://doi.org/10.1016/j.neuron.2010.01.034 PMID: 20223204.  
- Liu K, Sibille J, Dragoi G. Generative Predictive Codes by Multiplexed Hippocampal Neuronal Tuplets. Neuron. 2018; 99(6):1329–41. e6. https://doi.org/10.1016/j.neuron.2018.07.047 PMID: 30146305  

使用海马体的序列重放功能，实现对运动序列的优化，最优路径重放的频率会更高。

## 主要工作

- 使用RNN模拟PFC，因为皮层连接大部分是局部而复发性的。
- 解决TSP问题中的两个假设：
  - 非最优轨迹（non-optimal trajectories）里，奖励之间的距离比近最优轨迹（near-optimal  trajectories）中奖励之间的距离大。在重放中持有轨迹出现较少，以此规则获得最优轨迹。
  - 在最近激活的位置细胞重放中，正向和反向轨迹同时被编码。反向的重播允许模型在正方向和反方向上利用信息，但实际运动中，小鼠只会进行正向的运动。
- 面对的挑战：
  - 从无序的片段路径中学习全局的place cell激活序列
  - 将多个非最优的序列高效连接成一个通往目标位置的路径
  - 正向学习轨迹并反向生成它。

- 海马和PFC的连接证据：Shin JD, Jadhav SP. Multiple modes of hippocampal–prefrontal interactions in memory-guided behavior. Current opinion in neurobiology. 2016; 40:161–9. https://doi.org/10.1016/j.conb.2016.07.015 PMID: 27543753

## 方法

### Place-cell

- 基本设置：
  - 时间步N
  - 轨迹$$L(t_1\rightarrow t_N)$$ 
  - 位置$$S=(x,y)$$
  - 2D高斯位置场：$$f_k(s)=e^{-\frac{\Vert s-c_k\Vert^2}{w_k}}$$ ，$$k$$是位置细胞序号，$$f_k(s)$$是第k个位置细胞的平均发放率，$$w_k=\frac{r_k^2}{-\log(\Theta)}$$是常数，用来约束位置神经元的最高激活大部分包含在以$$$c_k$$为中心，$$r_k$$为半径的圆中
  - $$r_k$$是第$$k$$个位置场的半径
  - $$\Theta$$是控制位置细胞空间选择半径阈值
- 通过K个径向基函数来实现定位：$$X_{in}(t_n)=\{f_k(L(t_n))\}_{k\in 1 \cdot\cdot\cdot K}$$
- place cell可以进一步调研，这个跟脑内GPS有关（还有grid cell, head direction cell）

### Hippocampus replay

- 参考文献：Carr MF, Jadhav SP, Frank LM. Hippocampal replay in the awake state: a potential substrate for memory consolidation and retrieval. Nature neuroscience. 2011; 14(2):147–53. https://doi.org/10.1038/nn. 2732 PMID: 21270783  

- random replay generative model   
- a snippet $$S(n;s)=X_{in}(t_n \rightarrow t_{n+s})$$  

### Reward propagation











![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20200925-1.png)

## 结论

上丘脑到丘脑底部回路superior colliculus–subthalamus (SC–S) pathway 是决定hunting的神经基础
