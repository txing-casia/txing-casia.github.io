---
layout:     post
title:      "Reinforcement Learning | A survey of deep RL and IL for autonomous driving policy learning"
subtitle:   ""
date:       2022-05-31
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
    - HRL
---

# A survey of deep RL and IL for autonomous driving policy learning

论文链接：https://arxiv.org/abs/2101.01993v1

## 背景

- 首先介绍5类结合了IL和RL的自动驾驶模型。First, a taxonomy of the literature studies is constructed from the system perspective, among which five modes of integration of DRL/DIL models into an AD architecture are identified. 

- 其次介绍自动驾驶中具体的RL和IL任务和公式。Second, the formulations of DRL/DIL models for conducting specified AD tasks are comprehensively reviewed, where various designs on the model state and action spaces and the reinforcement learning rewards are covered. 

- 最后介绍RL和IL如何解决自动驾驶模型与参与者和环境交互的安全问题。Finally, an in-depth review is conducted on how the critical issues of AD applications regarding driving safety, interaction with other traffic participants and uncertainty of the environment are addressed by the DRL/DIL models. 

- **task-driven** and **problem-driven** perspectives

- 代表性文章：
  - [1] C. Urmson and W. Whittaker, “Self-driving cars and the urban challenge,” IEEE Intelligent Systems, vol. 23, no. 2, pp. 66–68, 2008.
  - [2] S. Thrun, “Toward robotic cars,” Communications of the ACM, vol. 53, no. 4, pp. 99–106, 2010.
  - [3] A. Eskandarian, Handbook of intelligent vehicles. Springer, 2012, vol. 2.
  - [4] S. M. Grigorescu, B. Trasnea, T. T. Cocias, and G. Macesanu, “A survey of deep learning techniques for autonomous driving,” J. Field Robotics, vol. 37, no. 3, pp. 362–386, 2020.

- 驾驶策略基于多个等级的抽象（multiple levels of abstraction），例如行为规划、运动规划和控制（behavior planning, motion planning and control）

- 典型的自动驾驶模型结构图：

  ![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220602-1.png)

- 相关综述的调研：
  - [13, 15] survey the motion planning and control methods of automated vehicles before the era of DL.
  - [29–33] review general DRL/DIL methods without considering any particular applications. 
  - [4] addresses the deep learning techniques for AD with a focus on perception
    and control, while [34] addresses control only. 
  - [35] provides a taxonomy of AD tasks to which DRL models have been applied and highlights the key challenges.

- pomdp相关参考文献：
  - G. Shani, J. Pineau, and R. Kaplow, “A survey of point-based pomdp solvers,” Autonomous Agents and Multi-Agent Systems, vol. 27, no. 1, pp. 1–51, 2013. 
  - W. S. Lovejoy, “A survey of algorithmic methods for partially observed markov decision processes,” Annals of Operations Research, vol. 28, no. 1, pp. 47–65, 1991.

- 强化学习（reinforcement learning）和模仿学习（imitation learning）算法分类示意图：

  ![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220620-1.png)

- learning from demonstrations (LfD)

- **模仿学习**的形式：
  - A demonstration dataset $$D=\{\xi_i \}_{i=0,...,N}$$,表示一系列的轨迹
  - $$\xi_i=\{(s^i_t,a^i_t)\}_{t=1,...,T}$$是state-action pairs（状态行为对）的序列
  - 专家策略$$\pi_E$$
  - 待优化的模仿策略$$\pi^*$$
  - $$\pi^*=\arg\min_{\pi} \mathbb{D}(\pi_E,\pi)$$
  - $$\mathbb{D}$$是策略间的相似性度量函数

- 模仿学习的三个分类：
  - **Behavior Clone (BC) :**
    
    - $$\min_{\theta} \mathbb{E}\mid\mid\pi_{\theta}-\pi_E\mid\mid_2$$
    
    - $$J(\theta)=\mathbb{E}_{(s,a)\sim D}[(\pi_{\theta}(s)-a)^2]$$
    
    - BC在训练集中表现良好，但在泛化性上表现差，covariate shift [66, 67]
    
    - 代表方法：
    
      - **DAgger**——S. Ross, G. J. Gordon, and D. Bagnell, “A reduction of imitation learning and structured prediction to no-regret online learning,” in International Conference on Artificial Intelligence and Statistics, ser. JMLR Proceedings, vol. 15, 2011, pp. 627–635.
    
      - **SafeDAgger**——J. Zhang and K. Cho, “Query-efficient imitation learning for end-to-end autonomous driving,” arXiv preprint arXiv:1605.06450, 2016.
    
  - **Inverse Reinforcement Learning (IRL)：**
  
    - $$\max_{\theta} \mathbb{E}_{\pi_E}[G_t|r_{\theta}]-\mathbb{E}_{\pi}[G_t|r_{\theta}]$$
  
    - $$J(\theta)=\mathbb{E}_{\xi_i \sim D}[\log P(\xi_i|r_{\theta})]$$
  
    - 代表方法：
  
      - **guided cost learning (GCL)**——C. Finn, S. Levine, and P. Abbeel, “Guided cost learning: Deep inverse optimal control via policy optimization,” in International Conference on Machine Learning, 2016, pp. 49–58.
  
        （it handles unknown dynamics in high-dimensional complex systems and learns complex neural network cost functions through an efficient sample-based approximation.）
  
  -  **Generative Adversarial Imitation Learning (GAIL):**
  
    - Generative adversarial imitation learning (GAIL) [81] directly learns a policy from expert demonstrations while requiring neither the **reward design** in RL nor the **expensive RL process** in the inner loop of IRL.
    - $$\min_{\pi_{\theta}}\max_{D_{\omega}} \mathbb{E}_{\pi_{\theta}}[\log D_{\omega}(s,a)]+\mathbb{E}_{\pi_E}[\log(1-D_{\omega}(s,a))]-\lambda H(\pi_{\theta})$$，其中$$H(\pi)$$是一个正则熵项，生成器和判别器通过下式更新：
    - $$\nabla_{\theta}J(\theta)=\mathbb{E}_{\pi}[\nabla_{\theta} \log \pi_{\theta}(a\mid s)Q(s,a)]-\lambda\nabla_{\theta}H(\pi_{\theta})$$
    - $$\nabla_{\omega}J(\omega)=\mathbb{E}_{\pi}[\nabla_{\omega} \log D_{\omega}(s,a)]+\mathbb{E}_{\pi_E}[\nabla_{\omega} \log(1-D_{\omega}(s,a))]$$
    - Fu et al.[84] proposed adversarial inverse reinforcement learning (AIRL) based on an adversarial reward learning formulation, which can recover reward functions that are robust to dynamics changes. 
      - J. Fu, K. Luo, and S. Levine, “Learning robust rewards with adversarial inverse reinforcement learning,” CoRR, vol. abs/1710.11248, 2017.

- AD（Autonomous driving）系统的几个模块：
  - 









## 主要内容

- 





## 总结

