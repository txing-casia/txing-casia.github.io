---
layout:     post
title:      "Reinforcement Learning | Proximal Policy Optimization Algorithms"
subtitle:   "PPO"
date:       2021-06-29
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
---

# Proximal Policy Optimization Algorithms

论文链接：https://arxiv.org/pdf/1707.06347.pdf

## 背景

基于策略梯度和 trust region policy optimization (TRPO)方法，提出multiple epochs of minibatch updates的方式，解决了Policy Gradient算法中步长难以确定的问题。而一般策略梯度方法 perform one gradient update per data sample。

## 主要内容

-  目的是实现数据高效和鲁棒性（data efficient, and robust）
-  使用 first-order optimization
- PPO有两种主要变体：PPO-penalty和PPO-clip。
  - PPO-Penalty 近似解决了像TRPO这样的受KL约束的更新，但是对目标函数中的KL偏离进行了惩罚，而不是使其成为硬约束，并且在训练过程中自动调整了惩罚系数，以便对其进行适当缩放。
  - PPO-Clip 在目标中没有KL散度项，也没有任何约束。 取而代之的是依靠对目标函数的专门裁剪来消除新政策消除旧政策的激励。

$$
L(s,a,\theta_k,\theta)=\min \Bigg(\frac{\pi_{\theta}(a|s)}{\pi_{\theta_k}(a|s)}A^{\pi_k}(s,a), \text{clip}\bigg(\frac{\pi_{\theta}(a|s)}{\pi_{\theta_k}(a|s)},1-\epsilon,1+\epsilon \bigg)A^{\pi_k}(s,a) \Bigg)
$$




## 总结

相比于TRPO，PPO缺失比较简单、直接，特别是PPO2（PPO-Clip），通过裁减损失哈数的方式保证了更新的稳定。












