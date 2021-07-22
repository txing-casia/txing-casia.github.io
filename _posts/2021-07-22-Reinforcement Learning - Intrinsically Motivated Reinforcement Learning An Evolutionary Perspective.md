---
layout:     post
title:      "Reinforcement Learning | Intrinsically Motivated Reinforcement Learning: An Evolutionary Perspective"
subtitle:   ""
date:       2021-07-22
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
    - HRL
    - Reward Shaping
---

# Intrinsically Motivated Reinforcement Learning: An Evolutionary Perspective

论文链接：https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=5471106

## 主要内容

- intrinsically motivated 这个词最开始是在1950年的文章中提出的，文章认为需要有一种内在的操纵驱动力来解释为什么猴子会在没有任何外在奖励的情况下精力充沛地持续工作数小时来解决复杂的机械难题。内在动机行为对智力增长至关重要

- 本文解决了如何在人工系统中实现类似于内在动机的过程的问题，特别关注可能会或可能不会区分内在动机和外在动机的因素，其中外在动机是指由行为的特定奖励结果产生的动机，而不是行为本身。

- 相关工作：

  - 定义有趣度：Lenat's AM system [18], for example, focused on heuristic definitions of “interestingness,” 

  - 建立好奇心框架：Schmidhuber [32] [33] [34] [35] [36] [37] introduced methods for implementing forms of curiosity using the framework of computational reinforcement learning (RL)1 [47].

  - 奖励函数设计：Other researchers have reported interesting results of computational experiments involving evolutionary search for RL reward functions [1], [8], [19], [31], [43], but they did not directly address the motivational issues on which we focus.

  - 寻找本质奖励：Uchibe and Doya [51] do address intrinsic reward in an evolutionary context, but their aim and approach differ significantly from ours.

    [51]. E. Uchibe and K. Doya, "Finding intrinsic rewards by embodied evolution and constrained reinforcement learning", *Neural Netw.*, vol. 21, no. 10, pp. 1447-1455, 2008.

  - 与我们最接近的研究是 Elfwing 等人的研究。[11]其中使用遗传算法来搜索塑造奖励[23]和其他提高 RL 学习系统性能的学习算法参数。

    [11]. S. Elfwing, E. Uchibe, K. Doya and H. I. Christensen, "Co-evolution of shaping rewards and meta-parameters in reinforcement learning", *Adapt. Behav.*, vol. 16, pp. 400-412, 2008.



![Learning Intrinsic Rewards for Policy Gradient](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20210722-1.png)





## 总结



