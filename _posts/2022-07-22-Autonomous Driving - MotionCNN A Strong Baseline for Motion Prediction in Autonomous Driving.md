---
layout:     post
title:      "Autonomous Driving | MotionCNN: A Strong Baseline for Motion Prediction in Autonomous Driving"
subtitle:   ""
date:       2022-07-22
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## MotionCNN: A Strong Baseline for Motion Prediction in Autonomous Driving  

### 1 Introduction

- Motion prediction 目前仍然是一个困难的任务，本文提出了一个可以进行多模式的行为预测（multimodal motion prediction）的较强的baseline (based purely on Convolutional Neural Networks
  (CNNs)  
- 算法在 2021 Waymo Open Dataset Motion Prediction Challenge 取得第三名。相关代码见[Github](https://github.com/kbrodt/waymo-motion-prediction-2021)
- 运动预测任务最重要的一些方法：
  - birds-eye-view rasterized scene representations [15, 6, 4, 11, 18, 14]
  - methods incarnated using graph neural networks [2, 8, 25]  

### 2 Method

#### 2.1 Rasterization

- 栅格化历史轨迹获得标准化的数据输入
- agent 始终位于图像中心
- agent 速度方向与x轴对齐

#### 2.2 Model

- 在ImageNet上预训练
- 真实的行为难以预测，因此输出K个不同的行为假设
- 输入：多通道栅格化图像
- 输出：预测的K条轨迹，并带有可信度$$c_1,c_2,...,c_k$$，可信度用softmax归一化，满足$$\sum_k c_k=1$$

#### 2.3 Loss function  

- 使用k个高斯分布，网络输出高斯分布的均值，将每个高斯的协方差固定为单位矩阵$$I$$

- 损失函数使用混合高斯分布的负的log似然度negative log-likelihood (NLL)，混合高斯由真实轨迹坐标定义  
- 给定ground truth轨迹：$$X^{gt}=[(x_1,y_1),...,(x_T,y_T)]$$
- 给定K个预测的假设轨迹：$$X_k=[(x_{k,1},y_{k,1}),...,(x_{k,T},y_{k,T})],k=1,...,K$$

- 计算预测高斯混合下真实轨迹的负对数概率，其均值等于预测轨迹，单位矩阵I为协方差:compute negative log probability of the ground truth trajectory under the predicted mixture of Gaussians with the means equal to the predicted trajectories and the identity matrix I as covariance: 
  $$
  L=-\log P(X^{gt})=-\log\sum_k c_k \mathcal{N}(X^{gt};\mu=X_k,\Sigma=I)
  $$

- 损失函数可以进一步分解为1维高斯的乘积：
  $$
  L=-\log \sum_k c_k\prod \mathcal{N}(x_t^{gt};x_{k,t},1)\mathcal{N}(y_t^{gt};y_{k,t},1)\\
  =-\log \sum_k \exp \bigg(\log(c_k)-\frac{1}{2}\sum_{t=1}^T (x_t^{gt}-x_{k,t})^2+(y_t^{gt}-y_{k,t})^2 \bigg)
  $$

- 建议的损失函数没有明确地惩罚产生非常接近的轨迹的模型。然而，根据经验，我们没有观察到模式崩溃，因为将所有的概率质量组合到一个模式中会导致更高的风险策略和更高的预测失误损失值。因此，优化建议的损失产生足够的多模态。

#### 2.4 Inference  

- 为未来轨迹提供了6个假设

### 3 Experiments  

#### 3.1 Dataset

- Waymo Open Motion Dataset [22, 7]  
  - [22]. Pei Sun, Henrik Kretzschmar, Xerxes Dotiwalla, Aurelien Chouard, Vijaysai Patnaik, Paul Tsui, James Guo, Yin Zhou, Yuning Chai, Benjamin Caine, Vijay Vasudevan, Wei Han, Jiquan Ngiam, Hang Zhao, Aleksei Timofeev, Scott Ettinger, Maxim Krivokon, Amy Gao, Aditya Joshi, Sheng Zhao, Shuyang Cheng, Yu Zhang, Jonathon Shlens, Zhifeng Chen, and Dragomir Anguelov. Scalability in perception for autonomous driving: Waymo open dataset, 2020. 3, 4  
  - [7]. Scott Ettinger, Shuyang Cheng, Benjamin Caine, Chenxi Liu, Hang Zhao, Sabeek Pradhan, Yuning Chai, Ben Sapp, Charles Qi, Yin Zhou, et al. Large scale interactive motion forecasting for autonomous driving: The waymo open motion dataset. arXiv preprint arXiv:2104.10133, 2021. 1, 2, 3, 4  
- Waymo数据包括：
  - object trajectories  
  - 3D maps for 103354 segments 
  - Each segment is a 20 seconds recording of an object trajectory at 10Hz  
  - map data for the area covered by the segment
  - A single sample comprises 1 second of history and 8 seconds of future data obtained by breaking the segments into 9-second windows with 5 second overlap  
  - 每个这样的样本包含多达8个标记为“有效”的代理，模型需要预测它们在未来8秒内的位置。Every such sample contains up to 8 agents marked as ”valid” for which the model needs to predict their positions for 8 seconds into the future.
- Rasterisation details  
  - 栅格尺寸：224 × 224 × (3 + 2T )，$$T=11$$​是快照（snapshot）数，3是RGB图像的3个通道，包括road lines, crosswalks, traffic lights等；每个历史快照通过两个额外通道表示：（1）The mask representing the location of the target agent. （2）the mask representing all other agents nearby  
  - 智能体位于坐标$$[61,112]$$，其速度与图像的X轴对齐

#### 3.2 Metrics



​	
