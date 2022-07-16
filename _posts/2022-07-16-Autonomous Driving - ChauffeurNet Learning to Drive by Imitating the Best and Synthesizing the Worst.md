---
layout:     post
title:      "Autonomous Driving | ChauffeurNet: Learning to Drive by Imitating the Best and Synthesizing the Worst (Waymo, 2018)"
subtitle:   ""
date:       2022-07-16
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## ChauffeurNet: Learning to Drive by Imitating the Best and Synthesizing the Worst

### 1. Introduction

- 提出通过扰动专家的驾驶数据创建更多有趣的驾驶情景，例如碰撞和off-road等。

- 在模仿学习中，并不是模仿所有的数据，而是利imitation loss惩罚不期望的情况。

- 数据：30 million real-world expert driving examples, corresponding to about 60 days of continual driving  
- 由于无论是原始传感器输入还是直接的控制器输出都很难产生扰动，因此在mid-level的输入输出数据上实施扰动。

### 2. Related Work

