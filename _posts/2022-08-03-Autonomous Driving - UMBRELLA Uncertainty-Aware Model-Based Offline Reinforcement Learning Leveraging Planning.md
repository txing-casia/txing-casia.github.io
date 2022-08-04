---
layout:     post
title:      "Autonomous Driving | UMBRELLA Uncertainty-Aware Model-Based Offline Reinforcement Learning Leveraging Planning"
subtitle:   ""
date:       2022-08-03
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## UMBRELLA: Uncertainty-Aware Model-Based Offline Reinforcement Learning Leveraging Planning

- 在学习过程中考虑了随机不确定性的影响，提高了模型的可迁移性和可解释性

### 1 Introduction  

- 目前的模型考虑到多智能体之间的交互和复杂行为，多采用工程设计的驾驶策略，但这样难以适用更复杂的任务。强化学习通过试错学习的方式避免了这些手工设计，但需要大量的试错机会。相比于模仿学习习得次优行为，强化学习可以提升不同类型数据的质量

- 主要贡献：
  - 提出了一个基于模型的离线规划算法UMBRELLA，在观测上考虑了认知和偶然的不确定性（epistemic 和 aleatoric）  
  - 引入了一个消融实验，优化最坏情况模型
  - 实验中，在稠密车流（with dense traffic）的城市和高速场景中，胜过了BC行为克隆方法和MBOP算法

### 2 Related Work  

- **Model-based Offline Reinforcement Learning**：

  其主要的问题是the distributional shift。MOReL和MBOP方法将动态模型的认知不确定性估计结合到奖励函数中，以惩罚未被行为分布覆盖的状态

  - Sergey Levine, Aviral Kumar, George Tucker, and Justin Fu. Offline reinforcement learning: Tutorial, review, and perspectives on open problems. CoRR, 2020. arXiv:2005.01643.  

  这些方法在但智能体环境中测试，因此没考虑到由行人和其它车辆行为造成的偶然不确定性（aleatoric uncertainty）的影响。[Henaff et al., 2019]通过由条件变分自动编码器（conditional variational autoencoder，CVAE）表示的随机动力学模型解决了这个问题[Kingma and Welling, 2014]。然而，他们的方法依赖于策略学习，这与基于模型的离线规划相比，可解释性和控制灵活性有所降低。

  - Mikael Henaff, Alfredo Canziani, and Yann LeCun. Model-predictive policy learning with uncertainty regularization for driving in dense traffic. In International Conference on Learning Representations, \2019.  
  - Diederik P. Kingma and Max Welling. Auto-Encoding Variational Bayes. In International Conference on Learning Representations, 2014.  

- **Interaction-aware Motion Prediction and Planning**：

  单纯通过规划安全的pass进行自动驾驶忽略了车辆之间包含规划和预测（planning and prediction）的行为交互  

  引入博弈论的方法考虑了多智能体的动力学过程，但带来了计算上的开销。

  一些基于学习的模型可以生成更多的场景，但是没有考虑认知不确定性

  - Jerry Liu, Wenyuan Zeng, Raquel Urtasun, and Ersin Yumer. Deep structured reactive planning. In IEEE International Conference on Robotics and Automation, 2021.  
  - Nicholas Rhinehart, Jeff He, Charles Packer, Matthew A. Wright, Rowan McAllister, Joseph E. Gonzalez, and Sergey Levine. Contingencies from observations: Tractable contingency planning with learned behavior models. In IEEE International Conference on Robotics and Automation, 2021.  

### 3 The UMBRELLA Framework  

- UMBRELLA 是 MBOP 算法的扩展  
  - Arthur Argenson and Gabriel Dulac-Arnold. Model-based offline planning. In International Conference on Learning Representations, 2021.  

- 问题形式化，用MDP的$$(S; A; p; r; \gamma)$$ 表示，在offline设定中，智能体不与环境直接交互，而是从数据集中学习策略$$\pi_d$$。如果观测值$$O$$并不是完全可得，那么问题就变化为（partially observable MDP，POMDP），使用 $$M_{PO}=(S;A;O;p;r;\gamma)$$，处理该问题的常用方法是使用nth-order history 方法，可以近似得到状态估计，然后将其转变为MDP，用标准的RL方法处理。

- UMBRELLA使用连续的潜在变量（continuous latent variable）$$z_t\in Z$$ ，并枚举自车所有的可能行为（行为采样通过学到的BC policy）。预测N条长度界限为H的轨迹。还使用了一个return-weighted trajectory optimizer，处理POMDP问题，用持续观测状态从$$o_{t-_c:t}$$直到时间步t估计缺失的观测状态。
- 






### 总结

