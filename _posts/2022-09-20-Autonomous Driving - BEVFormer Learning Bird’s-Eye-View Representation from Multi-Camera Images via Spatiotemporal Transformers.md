---
layout:     post
title:      "Autonomous Driving | BEVFormer: Learning Bird’s-Eye-View Representation from Multi-Camera Images via Spatiotemporal Transformers"
subtitle:   ""
date:       2022-09-20
author:     "Pcon"
header-img: "img/post-bg-py.jpg"
tags:
    - Autonomous Driving
---

## BEVFormer: Learning Bird’s-Eye-View Representation from Multi-Camera Images via Spatiotemporal Transformers

多相机3D检测的工作。提出了spatial cross-attention整合空间信息；提出了temporal self-
attention整合历史BEV信息。

代码开源：https://github.com/zhiqi-li/BEVFormer

### Related Work

**deformable attention mechanism:**

![deformable attention mechanism](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220920-1.png)

**Camera-based 3D Perception:**

过去的3D object detection 任务和map segmentation 任务是相互独立的。

1. predict the 3D bounding boxes based on 2D bounding boxes.

2. Another solution is to transform image features into BEV features and predict 3D bounding boxes from the top-down view.

从多相机特征生成BEV图。A straightforward method is converting perspective view into the BEV through Inverse Perspective Mapping (IPM) [35, 5]

### BEVFormer

- 几个特殊的设计：

  - **BEV queries;**

    BEV queries are grid-shaped learnable parameters, which is designed to query features in BEV space from multi-camera views via attention mechanisms.

  - **spatial cross-attention** and **temporal self-attention**;

    Spatial cross-attention and temporal self-attention are attention layers working with BEV queries, which are used to lookup and aggregate spatial features from multi-camera images as well as temporal features from history BEV, according to the BEV query.

#### BEV Queries

We predefine a group of grid-shaped learnable parameters $$Q\in R^{H\times W \times C}$$ as the queries of BEVFormer, where H, W are the spatial shape of the BEV plane. To be specific, the query $$Q_p \in R^{1\times C}$$ located at $$p = (x, y)$$ of $$Q$$ is responsible for the corresponding grid cell region in the BEV plane.

#### Spatial Cross-Attention

未来降低高昂的computational cost of vanilla multi-head attention [42]，本文采用了deformable attention [56]模型，这是一个资源高效的注意力层，其中每个BEV查询$$Q^p$$只与它在摄像机视图上感兴趣的区域交互。

spatial cross-attention (SCA)

![spatial cross-attention](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220922-1.png)

#### Temporal Self-Attention

temporal self-attention (TSA) layer

![temporal self-attention](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220922-2.png)

### 总结

后续没仔细看了，整体框架感觉各个模块衔接比较自然流畅，这也是目前最好的自动驾驶视觉感知方案。