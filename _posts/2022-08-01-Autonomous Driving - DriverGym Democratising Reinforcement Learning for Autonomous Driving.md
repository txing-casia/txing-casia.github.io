---
layout:     post
title:      "Autonomous Driving | DriverGym Democratising Reinforcement Learning for Autonomous Driving"
subtitle:   ""
date:       2022-08-01
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## DriverGym: Democratising Reinforcement Learning for Autonomous Driving    
- 现状：目前缺少open-source platform来用real-world data训练和高效地验证RL算法。
- DriverGym 平台的特点：
  - 开源（opensource）
  - 与Gym兼容（OpenAI Gym-compatible environment specifically tailored）
  - 超过1000小时专家记录数据（more than 1000 hours of expert logged data）  
  - 灵活的闭环评估协议（flexible closed-loop evaluation protocol）
  - 提供行为克隆baselines（provide behavior cloning baselines using supervised learning and RL）  
  - 代码：https://lyft.github.io/l5kit/  （已失效）

- 环境结构：

![DriverGym](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/20220801-2.png)

> DriverGym: an open-source gym environment that enables training RL driving policies on real-world data. The RL policy can access rich semantic maps to control the ego (**red**). Other agents (**blue**) can either be simulated from the data logs or controlled using a dedicated policy pre-trained on real-world data. We provide an extensible evaluation system (**purple**) with easily configurable metrics to evaluate the idiosyncrasies of the trained policies.  

### 1 Introduction  

- 自动驾驶开源RL仿真环境的对比

![自动驾驶开源RL仿真环境的对比](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/20220801-3.png)

- 对仿真平台的几个需求：
  - (1) 易于训练RL策略；be used to easily train RL policies using real-world logs, 
  - (2) 可仿真实际的和反应式的周围智能体行为反应；simulate surrounding agent behavior that is both realistic and reactive to the ego policy, 
  - (3) 可高效评估模型；effectively evaluate the trained models,
  - (4) 设计灵活可调；be flexible in its design, 
  - (5) 包容整个相关研究社区；inclusive to the entire research community,
- 使用了最大的公开数据集：Level 5 Prediction Dataset
  - J. Houston, G. Zuidhof, L. Bergamini, Y. Ye, A. Jain, S. Omari, V. Iglovikov, and P. Ondruska. One thousand and one hours: Self-driving motion prediction dataset. https://level-5.global/level5/data/, 2020.  
- 支持反应式的行为仿真
  - Luca Bergamini, Y. Ye, Oliver Scheel, Long Chen, Chih Hu, Luca Del Pero, Blazej Osinski, Hugo Grimmett, and Peter Ondruska. Simnet: Learning reactive self-driving simulations from real-world observations. ArXiv, abs/2105.12332, 2021.

- 闭环评估系统中提供了自动驾驶相关的评估指标，指标支持扩展和合并，应用于策略的训练

- 主要贡献：
  - An open-source and OpenAI gym-compatible environment for autonomous driving task;
  - Support for more than 1000 hours of real-world expert data;
  - Support for logged agents replay or data-driven realistic agent trajectory simulations;
  - Configurable and extensible evaluation protocol;
  - Provide pre-trained models and the corresponding reproducible training code.  

### 2 Related Work  

- **赛车仿真环境（Racing simulators）：**
  - **TORCS**：提供了受限的驾驶场景
    - E. Espié, Christophe Guionneau, Bernhard Wymann, Christos Dimitrakakis, Rémi Coulom, and Andrew Sumner. Torcs, the open racing car simulator. 2005.  
  - **Highway-Env**：环境与Gym兼容，但缺少交通灯、评估协议和专家数据
    - Edouard Leurent. An environment for autonomous driving decision-making. https://github.com/eleurent/highway-env, 2018.  
- **交通仿真环境（Traffic simulators）**：
  - **CARLA**：支持多变的交通情况训练和测试，但周围车辆使用手写规则（hand-coded rules），真实性有限。
    - A. Dosovitskiy, G. Ros, Felipe Codevilla, Antonio M. López, and V. Koltun. Carla: An open urban driving simulator. ArXiv, abs/1711.03938, 2017.  
  - **SUMO**：支持多变的交通情况训练和测试，但周围车辆使用手写规则（hand-coded rules），真实性有限。
    - Pablo Alvarez Lopez, Michael Behrisch, Laura Bieker-Walz, Jakob Erdmann, Yun-Pang Flötteröd, Robert Hilbrich, Leonhard Lücken, Johannes Rummel, Peter Wagner, and Evamarie Wießner. Microscopic traffic simulation using sumo. In The 21st IEEE International Conference on Intelligent Transportation Systems. IEEE, 2018. URL https://elib.dlr.de/124092/.  
  - **SMARTS** ：有`Social Agent Zoo`支持数据驱动周围车辆行为。 
    - Ming Zhou, Jun Luo, Julian Villela, Yaodong Yang, David Rusu, Jiayu Miao, Weinan Zhang, Montgomery Alban, Iman Fadakar, Zheng Chen, Aurora Chongxi Huang, Ying Wen, Kimia Hassanzadeh, Daniel Graves, Dong Chen, Zhengbang Zhu, Nhat M. Nguyen, Mohamed Elsayed, Kun Shao, Sanjeevan Ahilan, Baokuan Zhang, Jiannan Wu, Zhengang Fu, Kasra Rezaee, Peyman Yadmellat, Mohsen Rohani, Nicolas Perez Nieves, Yihan Ni, Seyedershad Banijamali, Alexander Cowen Rivers, Zheng Tian, Daniel Palenicek, Haitham Ammar, Hongbo Zhang, Wulong Liu, Jianye Hao, and Jintao Wang. Smarts: Scalable multi-agent reinforcement learning training school for autonomous driving. ArXiv, abs/2010.09776, 2020.  
  - **CRTS**：提供了logs数据接口，使用64小时的真实驾驶数据（real-world logs）训练周围车辆的行为。集成在Carla中。
    - Blazej Osinski, Piotr Milos, Adam Jakubowski, Pawel Ziecina, Michal Martyniak, Christopher Galias, Antonia Breuer, Silviu Homoceanu, and Henryk Michalewski. Carla real traffic scenarios - novel training ground and benchmark for autonomous driving. ArXiv, abs/2012.11329, 2020.  
  - **DriverGym**：支持反应式的agent使用数据驱动模型学习真实世界的数据，并提供了1000小时真实的驾驶记录可用于仿真agent

### 3 DriverGym  

- 模型兼容两个流行的框架训练策略：

  - **SB3**：[Stable Baselines3 (SB3)](https://github.com/DLR-RM/stable-baselines3)是 PyTorch 中强化学习算法的一组可靠实现。它是[Stable Baselines](https://github.com/hill-a/stable-baselines)的下一个主要版本。
    - Antonin Raffin, Ashley Hill, Maximilian Ernestus, Adam Gleave, Anssi Kanervisto, and Noah Dormann. Stable baselines3. https://github.com/DLR-RM/stable-baselines3, 2019.  
  - **RLlib**：RLlib是一个开源强化学习库,提供了高度可扩展能力和不同应用的统一的[API](https://so.csdn.net/so/search?q=API&spm=1001.2101.3001.7020)。RLlib原生支持Tensorflow，Tensorflow Eager，以及PyTorch，但其内部与这些框架无关。
    - Eric Liang, Richard Liaw, Robert Nishihara, Philipp Moritz, Roy Fox, Ken Goldberg, Joseph E. Gonzalez, Michael I. Jordan, and Ion Stoica. Rllib: Abstractions for distributed reinforcement learning. In ICML, 2018.  

- 例子场景：绿线是策略预测的轨迹

  ![Visualization of an episode rollout (ego in red, agents in blue) in DriverGym. The policy
  prediction (green line) is scaled by factor of 10 and shown at 2 second intervals for better viewing](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/20220801-4.png)

- action space：行为为 $$(x; y; yaw)$$ (yaw是朝向)，用于更新ego-agent行为；环境输出 $$(acceleration; steer)$$ 用于计算下一时刻的observation
- reactive agent：允许周围车辆使用两种方式控制：
  - log replay：回放真实世界中收集的数据；
  - reactive simulation：可使用真实世界数据训练neural-network-based agent models，用于控制车辆行为
- reward：支持不同的自动驾驶评价指标，在每一帧进行评价计算，指标可以整合为奖励函数。



### 总结

整体来看，支持Gym环境大大方便了仿真和调试，一些细节问题由于没有实际使用该环境还不清楚，比如车辆密度、速度、周围车辆的观测质量、轨迹质量等。

本文作者正对更多细粒度场景设计评估方案，例如或绿灯路口重新启动等。

