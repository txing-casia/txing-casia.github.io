---
layout:     post
title:      "Network Structure | A ConvNet for the 2020s (Facebook, 2022)"
subtitle:   ""
date:       2022-07-22
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Network Structure
    - Autonomous Driving
---

## A ConvNet for the 2020s

- 纯卷积的网络堪比transformer。

Vision Transformers (ViTs) 的引入迅速取代了ConvNets，成为最先进的图像分类模型。另一方面，普通的 ViT 在应用于目标检测和语义分割等一般计算机视觉任务时面临困难。分层 Transformers（例如，Swin Transformers）重新引入了几个 ConvNet 先验，使 Transformers 作为通用视觉骨干实际上可行，并在各种视觉任务上表现出卓越的性能。然而，这种混合方法的有效性在很大程度上仍归功于 Transformer 的内在优势，而不是卷积固有的归纳偏差。在这项工作中，我们重新检查了设计空间并测试了纯 ConvNet 所能达到的极限。

我们逐渐将标准 ResNet “现代化”为视觉 Transformer 的设计，并在此过程中发现了导致性能差异的几个关键组件。这一探索的结果是一系列纯 ConvNet 模型，称为 ConvNeXt。ConvNeXts 完全由标准 ConvNet 模块构建，在准确性和可扩展性方面与 Transformer 竞争，实现 87.8% ImageNet top-1 准确率，在 COCO 检测和 ADE20K 分割方面优于 Swin Transformers，同时保持标准 ConvNet 的简单性和效率。

### 1. Introduction

- 过去的十年视觉认知领域的研究从特征工程转移到了网络结构设计。
- solutions to numerous computer vision tasks in the past decade depended significantly on a sliding-window, fully- convolutional paradigm  

- The **biggest challenge** is ViT’s global attention design, which has a **quadratic complexity** with respect to the input size. This might be acceptable for ImageNet classification, but quickly becomes intractable with higher-resolution inputs.  

- **Swin Transformer** [45] is a milestone work in this direction, demonstrating for the first time that Transformers can be adopted as a generic vision backbone and achieve state-of-the-art performance across a range of computer vision tasks beyond image classification  
  - [45]. Ze Liu, Yutong Lin, Yue Cao, Han Hu, Yixuan Wei, Zheng Zhang, Stephen Lin, and Baining Guo. Swin transformer: Hierarchical vision transformer using shifted windows. 2021.  

### 总结

写作语言非常高级的一篇介绍网络结构的文章，介绍了如何构建超越transformer的纯卷积网络。并在计算机视觉多项任务中取得了成功，包括ImageNet top-1。文章值得一看，但由于这不是我重点关注方向，不详细介绍了。
