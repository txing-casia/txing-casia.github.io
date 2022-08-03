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
- 仿真的目标是迭代地生成观测状态序列$$\tau=\{s_1,s_2,...,s_T\}$$，然后计算车辆轨迹$$p_t$$，包括$$(x;y;\theta)$$
- $$s_{t+1}=S(s_t,a_t)$$，$$p_{t+1}=f(p_t,a_t)$$

### Imitation Learning Using a Differentiable Simulator  

- $$L(s_t,a_t)=\mid\mid \overline{p}_t - p_t\mid\mid_1$$
- $$J(\pi)=\mathbb{E}_{\overline{\tau}\sim\pi_E}\mathbb{E}_{\tau\sim\pi} \sum_{t}\gamma^t L(s_t,a_t)$$，$$\pi_E$$是专家策略，$$\pi$$是模型的策略，希望两个策略接近

![Imitation learning from expert demonstrations](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220803-2.png)

> P.S.：由于轨迹的开始阶段均来自专家策略，会引入bias，在策略更新的时候，在运动开始的第K步之后才计算梯度，以此避免bias

- 策略梯度的计算（用下标表示偏微分，$$\theta$$是策略参数）：
  $$
  J_s^t = L_s+L_a\pi_s+\gamma J^{t+1}_{\theta}(S_s+S_a\pi_{s})\\
  J_{\theta}^t = L_a\pi_{\theta}+\gamma(J_s^{t+1}S_a\pi_{\theta}+J_{\theta}^{t+1})
  $$
  > Ref: N. Heess, G. Wayne, D. Silver, T. Lillicrap, T. Erez, and Y. Tassa. Learning continuous control policies by stochastic value gradients. In Advances in Neural Information Processing Systems, \2015.  
### Experiments  

- Lyft Motion Prediction Dataset [6]：数据采集自加利福尼亚州帕洛阿尔托的复杂城市路线。数据集捕捉各种真实世界的情况，例如在多车道交通中驾驶、转弯、在十字路口与车辆互动等。
  - J. Houston, G. Zuidhof, L. Bergamini, Y. Ye, A. Jain, S. Omari, V. Iglovikov, and P. Ondruska. One thousand and one hours: Self-driving motion prediction dataset. Conference on Robot Learning (CoRL), 2020.  
- 模型在100小时子集上训练，并在25小时子集上测试。

- three state-of-the-art baselines：
  - Naive Behavioral Cloning (BC)  
  - Behavioral Cloning + Perturbations (BC-perturb)  
    - M. Bansal, A. Krizhevsky, and A. Ogale. Chauffeurnet: Learning to drive by imitating the best and synthesizing the worst. 12 2018.  
  - Multi-step Prediction (MS Prediction)  
    - A. Venkatraman, M. Hebert, and J. Bagnell. Improving multi-step prediction of learned time series models. In AAAI, 2015.  

![性能对比](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220803-3.png)

> 指标值越小越好，本文模型取得最好的表现以及最低的l1K指标（综合其它指标，每1000英里干预次数）

- 评价指标：
  - **L2**: L2 distance to the underlying expert position in the driving log in meters.
  - **Off-road events**: we report a failure if the planner deviates more than 2m laterally from the reference trajectory – this captures events such as running off-road and into opposing traffic.
  - **Collisions**: collisions of the SDV with any other agent, broken down into front, side and rear collisions w.r.t. the SDV.
  - **Comfort**: we monitor the absolute value of acceleration, and raise a failure should this exceed 3 m/s2.
  - **I1K**: we accumulate safety-critical failures (collisions and off-road events) into one key metric for ease of comparison, namely Interventions per 1000 Miles (I1K)  

![仿真结果](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220803-4.png)


### 总结

策略梯度的推导部分可以继续看看，本文有仿真和实车实验，但方法对比上，对其它算法进行了修改，因此并不完整。
