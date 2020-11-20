---
layout:     post
title:      "Neuroscience | Generative Predictive Codes by Multiplexed Hippocampal Neuronal Tuplets"
subtitle:   ""
date:       2020-11-19
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Neuroscience
---

# Generative Predictive Codes by Multiplexed Hippocampal Neuronal Tuplets

论文链接：https://www.sciencedirect.com/science/article/pii/S0896627318306457

## 背景

利用神经元连音机制（neuronal ensemble mechanisms: high-repeat sequential neuronal ‘‘tuplets’’ operating），将大的序列拆分，在导航任务中快速学习。

- 内部生成的自发神经动力学internally generated spontaneous neuronal dynamics

- predictive coding theory（数学比较多可以看一看Heeger, D.J. (2017). Theory of cortical function. Proc. Natl. Acad. Sci. USA 114, 1773–1782.）

## 主要工作

- 新的路径表征并非从头学起；Hippocampal Network place cells接收外部刺激，结合内部生成模型形成对新的连续快速内部表征
- 将连音作为经验和环境信息存储，实现高效学习和存储
- 三种路径信息的内部表示存储模式：
  - 外部驱动模式：完整存储每一条路径的内部表示（占用的存储空间大，利用效率低）
  - 内部连音模式：将路径表征的子段存储，每段之间可以学习组合（占用的存储空间小，利用效率高）
  - 内部、外部混合模式：有些路径完整存储，有些使用连音模式存储（占用的存储空间很小，利用效率很高）


## 总结

有一些启发，可惜模型是给的统计学公式。后面可以尝试将连音机制利用一下





