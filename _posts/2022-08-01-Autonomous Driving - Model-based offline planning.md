---
layout:     post
title:      "Autonomous Driving | Model-based offline planning"
subtitle:   ""
date:       2022-08-01
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## Model-based offline planning
- 由于成本、安全性等因素，很多情况下不能够直接与系统交互来学习控制策略，因此，只能从记录的log数据中学习控制策略（offline reinforcement learning）。本文介绍了一种从log数据中学到超越成圣log数据的原策略的新策略的方法，命名为 model-based ofline planning (MBOP)。

### Introduction

- Offline reinforcement learning包括：
  - model-free方法：
    - Yifan Wu, George Tucker, and Ofir Nachum. Behavior regularized offline reinforcement learning. CoRR, abs/1911.11361, 2019. URL http://arxiv.org/abs/1911.11361.  
    - Xue Bin Peng, Aviral Kumar, Grace Zhang, and Sergey Levine. Advantage-weighted regression: Simple and scalable off-policy reinforcement learning. arXiv preprint arXiv:1910.00177, 2019.  
    - Scott Fujimoto, David Meger, and Doina Precup. Off-policy deep reinforcement learning without exploration. In International Conference on Machine Learning, pp. 2052–2062, 2019.  
    - Ziyu Wang, Alexander Novikov, Konrad Zolna, Josh S Merel, Jost Tobias Springenberg, Scott E Reed, Bobak Shahriari, Noah Siegel, Caglar Gulcehre, Nicolas Heess, et al. Critic regularized regression. Advances in Neural Information Processing Systems, 33, 2020.  
  - model-based方法：MOPO, MoREL学习一个模型，然后用于训练一个无模型策略，这种模式和Dyna模式类似。
    - **MOPO**: Tianhe Yu, Garrett Thomas, Lantao Yu, Stefano Ermon, James Zou, Sergey Levine, Chelsea Finn, and Tengyu Ma. Mopo: Model-based offline policy optimization. arXiv preprint arXiv:2005.13239, 2020.  
    - **MoREL**: Rahul Kidambi, Aravind Rajeswaran, Praneeth Netrapalli, and Thorsten Joachims. Morel: Modelbased offline reinforcement learning. arXiv preprint arXiv:2005.05951, 2020.  
    - **Dyna**: Richard S Sutton and Andrew G Barto. Reinforcement learning: An introduction. MIT press, 2018.  

- 本文的算法属于model-based，利用model-predictive control (MPC) ，扩展MPPI轨迹规划器，并使用实时规划，产生目标或满足奖励条件的策略。
  - Grady Williams, Nolan Wagener, Brian Goldfain, Paul Drews, James M Rehg, Byron Boots, and Evangelos A Theodorou. Information theoretic mpc for model-based reinforcement learning. In 2017 IEEE International Conference on Robotics and Automation (ICRA), pp. 1714–1721. IEEE, 2017b.  

- 本文模型MBOP包含三个要素：
  - a learnt **world model**, 
  - a learnt **behavior-cloning policy**, 
  - a learnt **fixed-horizon value-function**.  
- MBOP的核心优势是**数据高效**和**自适应**。只需仅100秒就可以训练出一个和奖励函数、目标状态、基于状态的约束相适应的策略。
- MBOP能够对非平稳目标和约束执行zero-shot自适应，但是没有处理非平稳动力学特性的机制。

### Model-based offline planning

- 描述问题为Markov Decision Process (MDP)，$$(S,A,p,r,\gamma)$$
  - $$s$$是系统状态
  - $$a$$是行为
  - $$p(s_{t+1}\mid s_t,a_t)$$是状态转移概率
  - $$r(s_t,a_t,s_{t+1})$$是奖励
  - $$\gamma=1$$是时间折扣系数
- MBOP包括三个函数近似器：
  - $$f_m$$：环境动力学的单步模型，$$(\hat{r}_t,\hat{s}_{t+1})=f_m(s_t,a_t)$$，本文使用$$f_m(s_t,a_t)_s$$表示状态预测，使用$$f_m(s_t,a_t)_r$$表示奖励预测。
  - $$f_b$$：表示一个行为克隆网络，$$a_t=f_b(s_t,a_{t-1})$$，被规划算法用来引导轨迹采样的先验。
  - $$f_R$$：是一个阉割的值函数，提供在状态s中采取行为a后，在固定界限$$R_H$$上的收益。$$\hat{R}_H = f_R(s_t,a_{t-1})$$

- MBOP-POLICY
  - 使用MPC输出每个新状态下的行为（$$a_t=\pi(s_t)$$）。MPC在每一时间步执行一个固定长度的规划，返回长度为H的轨迹T。选择该轨迹的第一个行为$$a_t$$并返回。

![High-Level MBOP-Policy](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220802-1.png)

- MBOP-TRAJOPT
  - 在PDDM的基础上增加一个策略先验$$f_b$$和价值预测$$f_R$$

![MBOP-Trajopt](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220802-2.png)

> P.S.：第11行在$$f_b$$给出的行为上加权了采样轨迹的行为，其含义可能是希望在网络没有收敛时，记录下来的行为也不要偏差太大，都保持在采样轨迹附近，参数$$\beta$$可被视为学习率。同时也便于第17行给出多条轨迹的平均输出（re-weighting）

### Experimental results

- 首先，在非常少的数据中心训练，其次，再迁移到基于相同系统动力学的两种novel tasks中：
  - **goal-conditioned tasks** (that ignore the original reward function)  
  - **constrained tasks** (that require optimising for the original reward under some state constraint)  

- 使用的数据集RL Unplugged (RLU) 和 D4RL 

  - **RL Unplugged (RLU)**：Caglar Gulcehre, Ziyu Wang, Alexander Novikov, Tom Le Paine, Sergio Gomez Colmenarejo, Kon- ´rad Zolna, Rishabh Agarwal, Josh Merel, Daniel Mankowitz, Cosmin Paduraru, et al. Rl unplugged: Benchmarks for offline reinforcement learning. arXiv preprint arXiv:2006.13888, 2020.  
    - cartpole-swingup
    - walker
    - quadruped  

  - **D4RL**：Justin Fu, Aviral Kumar, Ofir Nachum, George Tucker, and Sergey Levine. D4rl: Datasets for deep data-driven reinforcement learning. arXiv preprint arXiv:2004.07219, 2020.  
    - halfcheetah
    - hopper
    - walker2d
    - Adroit  

- 对于 RLU 中的 Quadruped 和 Walker 任务，由于数据集中性能高方差，在训练 $$f_b$$ 和 $$f_R$$ 的过程中，通过设定阈值，舍弃了性能不好的数据。 使用未过滤的数据来训练 $$f_s$$

- 对于所有的数据集，90%用于训练，10%用于测试验证

- 性能：For the RLU datasets (Fig. 1), we observe that MBOP is able to find a near-optimal policy on most dataset sizes in Cartpole and Quadruped with as little as **5000 steps**, which corresponds to **5 episodes**, or approximately 50 seconds on Cartpole and 100 seconds on Quadruped. On the Walker datasets MBOP requires 23 episodes (approx. 10 minutes) before it finds a reasonable policy, and with sufficient data converges to a score of 900 which is near optimal. On most tasks, MBOP is able to generate a policy significantly better than the behavior data as well as the the BC prior.  

- MBOP模型容易适应新的目标函数，例如添加新的子目标函数$$R'_n$$时，
  $$
  R'_n = \sum_t f_{obj}(s_t)
  $$
  其中，$$f_{obj}$$是用户自定义的目标函数。只需要将轨迹更新规则改为：
  $$
  T_t=\frac{\sum_{n=1}^N e^{kR_n+k_{obj}R'_n}A_{n,t}}{\sum_{n=1}^N e^{kR_n+k_{obj}R'_n}}
  $$

- 为了验证上述模型的适应能力，进行了两个实验：

  - **goal-conditioned control**（忽略原始奖励，$$k=0$$，学习新奖励）
  - **constrained control**（增加了state-based constraint，然后探索合适的 $$k$$ 和 $$k_{obj}$$ ）

![ZERO-SHOT TASK ADAPTATION](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220802-3.png)

### 总结

MBOP为策略生成提供了一种易于实施、数据高效、稳定且灵活的算法。

由于使用了在线规划，使其能够应对变化的目标、成本和环境限制。

但是算法没有在更复杂的场景和约束条件下测试，因此适用范围和效果还缺少验证。
