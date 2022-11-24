---
layout:     post
title:      "Autonomous Driving | Parallel, Angular and Perpendicular Parking for Self-Driving Cars using Deep Reinforcement Learning"
subtitle:   ""
date:       2022-09-30
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Autonomous Driving
    - Reinforcement Learning
---

## Parallel, Angular and Perpendicular Parking for Self-Driving Cars using Deep Reinforcement Learning

parking是所有驾驶行为的最终步骤，主要分为三类：垂直、平行、斜向泊车。

本文使用DDPG控制汽车完成三种场景的泊车。

### 1 Introduction

- 强化学习控制泊车：
  - "Reinforcement learning-based end-to-end parking for automatic parking system", Sensors (Switzerland), vol. 19 ,September 2019.
  - “An Automatic Parking Model Based on Deep Reinforcement Learning", 2021 J. Phys.: Conf. Ser. 1883 012111.
- parallel autonomous parking (truncated Monte Carlo tree):
  - “Data efficient reinforcement learning for integrated lateral planning and control in automated parking system”, Sensors (Switzerland), vol. 20, pp. 1–24, dec 2020.
  - "Reinforcement Learning-Based Motion Planning for Automatic Parking System," in IEEE Access, vol. 8, pp. 154485-154501, 2020, doi:  0.1109/ACCESS.2020.3017770.
- 避障：
  - "Q-Learning for Autonomous Mobile Robot Obstacle Avoidance," 2019 IEEE International Conference on Autonomous Robot Systems and Competitions (ICARSC), Porto, Portugal, 2019, pp. 1-7. DOI: 10.1109/ICARSC.2019.8733621
  - Combining YOLO and Deep Reinforcement Learning for Autonomous Driving in Public Roadworks Scenarios. In Proceedings of the 14th International Conference on Agents and Artificial Intelligence -
    Volume 3: ICAART, ISBN 978-989-758-547-0, pages 793-800. DOI: 10.5220/0010913600003116.




### 总结

本文除了比较新之外（2022）没有无亮点，完全借用DDPG分别实现三种库的parking
