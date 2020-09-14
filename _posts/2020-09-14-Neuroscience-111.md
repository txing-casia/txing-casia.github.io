---
layout:     post
title:      "Neuroscience | Vector-based navigation using grid-like representations"
subtitle:   ""
date:       2020-09-08
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Neuroscience
---

# Vector-based navigation using grid-like representations in artificial agent

论文链接：https://www.nature.com/articles/s41586-018-0102-6/

## 背景

RL在许多方面取得了好的成绩，但在哺乳动物的空间行为（mammalian spatial behaviour）的熟练度上，人工agent的性能仍然不足。 动物的空间感知由网格细胞（grid cell）支持，它提供了一种度量编码空间（a metric for coding space）的能力。本文模仿grid cell在DRL的基础上开发一种类哺乳动物的导航能力（mammal-like navigational abilities）。

被赋予了grid-like representations的agent在导航任务上性能超过了人类专家in challenging, unfamiliar,
and changeable environments，甚至有走捷径的能力。

## 主要工作

- 把速度信息作为输入给到一个LSTM循环神经网络，网络更新通过Backpropagation through time。LSTM映射位置和方向到一个线性层，在线性层中使用dropout，每个时刻随机50%的神经元被沉默。

- foraging behaviour觅食行为涉及到路径集成能力
- entorhinal network内嗅皮层网络帮助形成稳定的空间活动
- 实验效果如下图：

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20200914-1.png)

- vector-based navigation: 内嗅网格细胞产生目标驱动向量度量欧式空间，使得动物能够沿着一条直接路径到达目标





















