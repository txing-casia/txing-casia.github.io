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

![High-Level MBOP-Policy](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/20220802-1.png)

- MBOP-TRAJOPT
  - 在PDDM的基础上增加一个策略先验$$f_b$$和价值预测$$f_R$$

![MBOP-Trajopt](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/20220802-2.png)



### 总结

