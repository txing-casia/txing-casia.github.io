---
layout:     post
title:      "Autonomous Driving | Urban Driver: Learning to Drive from Real-world Demonstrations Using Policy Gradients"
subtitle:   ""
date:       2022-08-03
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## Urban Driver: Learning to Drive from Real-world Demonstrations Using Policy Gradients  

- 取得了城市驾驶场景中最好的效果（Urban driving scenarios）
- 数据：使用100小时的城市道路专家示教数据
- 不必添加复杂的状态扰动；
- 不必在训练中收集额外的同策略数据；

### Introduction

![本文的闭环训练算法概览](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220803-1.png)

- 工业界最好轨迹规划器文献：
  - H. Fan, F. Zhu, C. Liu, L. Zhang, L. Zhuang, D. Li, W. Zhu, J. Hu, H. Li, and Q. Kong. Baidu apollo em motion planner. ArXiv, 2018.  

- 本文主要贡献：
  - 复杂城市驾驶场景中，第一个证明了用策略梯度学习，可以从大量真实世界演示数据中学习模仿驾驶策略；
  - 一个新的可微分仿真器，可基于过去的数据进行闭环仿真，并通过时间的反向传播计算策略梯度，实现快速学习；
  - 单纯在仿真器中训练可在真实世界中控制自动驾驶车辆，优于其他方法；
  - 源码可得：https://planning.l5kit.org.  

### Related work

- **Trajectory-based optimization**：

  - 这是当前工业界的主流方法（a dominant approach）

  - 依赖手工定义的损失和奖励
  - 损失的优化可结合一系列经典的算法：
    - A* [11]
    - RRTs [12]
    - POMDP with solver [13]
    - dynamic programming [14]  
  - 整体上是依赖human engineering，而不是数据驱动
- **Reinforcement learning（RL）**：

  - 依赖仿真器的构造、精确编码和优化的奖励信号
    - S. Shalev-Shwartz, S. Shammah, and A. Shashua. Safe, multi-agent, reinforcement learning for autonomous driving. ArXiv, 2016.  
  - 手工编程的仿真器，不能还原真实的长尾场景
  - 本文直接通过 mid-level representations 从真实世界的 log 中构建仿真环境
- **Imitation learning (IL) and Inverse Reinforcement Learning (IRL)**：
  -  原始的行为克隆（Naive behavioral cloning）面临协变量偏移问题（covariate shift）
  -   Adversarial Imitation Learning [31, 32, 33]，还没有在自动驾驶场景使用
- **Neural Motion Planners**：  
  - 在[34]中，原始感觉输入和高清地图被用于估计未来可能的SDV位置的成本量。基于这些成本量，可以对轨迹进行采样，并且选择最低成本的轨迹来执行。这些方法目前没有在实车测试。
    - W. Zeng, W. Luo, S. Suo, A. Sadat, B. Yang, S. Casas, and R. Urtasun. End-to-end interpretable neural motion planner. Int. Conference on Computer Vision and Pattern Recognition (CVPR), \2019.  
    - S. Casas, A. Sadat, and R. Urtasun. Mp3: A unified model to map, perceive, predict and plan. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 14403–14412, 2021.  

- **Mid-representations and the availability of large-scale real-world AD datasets**：
  - J. Houston, G. Zuidhof, L. Bergamini, Y. Ye, A. Jain, S. Omari, V. Iglovikov, and P. Ondruska. One thousand and one hours: Self-driving motion prediction dataset. Conference on Robot Learning (CoRL), 2020.
  - M.-F. Chang, J. Lambert, P. Sangkloy, J. Singh, S. Bak, A. Hartnett, P. C. De Wang, S. Lucey, D. Ramanan, and J. Hays. Argoverse: 3d tracking and forecasting with rich maps supplementary material. Int. Conf. on Computer Vision and Pattern Recognition (CVPR).  
  - state-of-the-art solutions for motion forecasting [8, 9]  
    - [8] J. Gao, C. Sun, H. Zhao, Y. Shen, D. Anguelov, C. Li, and C. Schmid. Vectornet: Encoding hd maps and agent dynamics from vectorized representation. In Int. Conf. on Computer Vision and Pattern Recognition (CVPR), 2020.
    - [9] M. Liang, B. Yang, R. Hu, Y. Chen, R. Liao, S. Feng, and R. Urtasun. Learning lane graph representations for motion forecasting. 2020.  
- **Data-driven simulation**：
  - [23] created a photo-realistic simulator for training an end-to-end RL policy. 
  - [5] simulated a bird’s-eye view of dense traffic on a highway. 
  - Finally, two recent works [39, 40] developed data-driven simulators and showed their usefulness for training and validating ML planners.   

  ### Differentiable Traffic Simulator from Real-world Driving Data

- 真实世界的经验轨迹：$$\overline{\tau}=\{\overline{s}_1,\overline{s}_2,...,\overline{s}_T\}$$
- 









### 总结

