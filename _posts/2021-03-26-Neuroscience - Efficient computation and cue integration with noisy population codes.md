---
layout:     post
title:      "Neuroscience | Efficient computation and cue integration with noisy population codes (Nature)"
subtitle:   "增益场"
date:       2021-03-26
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Neuroscience
    - Motor Primitives
---

# Efficient computation and cue integration with noisy population codes (Nature)

论文链接：https://www.nature.com/articles/nn0801_826

## 背景

- 单个的神经元活动包含噪声，因此不可靠。但是神经元的集群会体现出极大的可靠性。

- 目前有两种关于神经元群体的计算方式：function approximation 和 cue integration

- 多维吸引子的基函数网络可以将有噪声的神经元带入计算 basis function networks with multidimensional attractors
- Moreover, neurons in the intermediate layers of our model show response properties similar to those observed in several multimodal cortical areas (多峰皮层区域)

## 主要工作

- 感觉运动转换时非线性的

  Pouget, A. & Snyder, L. Computational approaches to sensorimotor transformations. *Nat. Neurosci.* 3, 1192–1198 (2000).


- 皮层中的神经元主要负责编码这些非线性的感觉-运动转换过程。其中主要涉及钟形调制曲线的神经元。
- 我们证明了一种特殊类型的神经网络，即具有**多维吸引子的基函数网络**，可以为有效地实现**非线性变换**和**与噪声神经元的提示集成**提供通用的体系结构。
- 网络结构示意：

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20210326-1.png)



## 总结

- 文章比较早也比较简单，图片挺好，说明了编码的方式，可以使用



