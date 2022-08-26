---
layout:     post
title:      "Autonomous Driving | MP3: A Unified Model to Map, Perceive, Predict and Plan (Uber ATG, 2021)"
subtitle:   ""
date:       2022-08-25
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## MP3: A Unified Model to Map, Perceive, Predict and Plan (Uber ATG, 2021)

- HD map具有的语义和几何信息使其成为自动驾驶系统的关键部件。但HD map的成本很高，难扩展，尤其是厘米级精度（centimeter-level accuracy）的情况下。因此能摆脱HD Map（地图加载失败、地图老旧等）的算法值得研究。本文提出了一种**end2end**的**不依赖地图**的自动驾驶算法——MP3。
- 输入为：
  - **raw sensor data**
  - **high-level command** (e.g., turn left at the intersection)
- 本文的定位为：mapless technology 的自动驾驶

### 1 Introduction

- 没有HD map的劣势：

  - 感知不能再依赖“人行道上的行人”、“道路上的车辆”这样的先验信息；

  - 进行规划的空间变大了

![有地图和无地图对比](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220826-1.png)

- 车辆需要把到达抽象成为路口直行（going straight at an intersection）、左转（turning left）和右转（turning right）等高阶的行为指令。
- 大多数的无地图方法模仿专家的驾驶行为（朝向角、加速度），但是没有提供可解释的中间表征（intermediate interpretable representations），而这可以帮助解释车辆的决策行为
  - End to end learning for self-driving cars. arXiv preprint arXiv:1604.07316, 2016.
  - End-to-end driving via conditional imitation learning. In ICRA, 2018.
  - Urban driving with conditional imitation learning. arXiv preprint arXiv:1912.00177, 2019.
  - Lift, splat, shoot: Encoding images from arbitrary camera rigs by implicitly unprojecting to 3d. In Proceedings of the European Conference on Computer Vision, 2020.
- 这些方法没有结构信息和先验知识，容易受到分布漂移（distributional shift）的影响
  - A reduction of imitation learning and structured prediction to no-regret online learning. In Proceedings of the fourteenth international conference on artificial intelligence and statistics, pages 627–635, 2011.
- 一些使用在线地图的方法（获得道路边界、中心线），要么过度简单（假设了车道是平行的，但这只在高速场景适用），要么难以将静态环境的不确定性纳入运动规划，而运动规划对于降低风险至关重要。[2, 16, 18, 21, 37],
  - Deep multi-sensor lane detection. In IROS, pages 3102–3109. IEEE, 2018.
  - 3d-lanenet: End-to-end 3d multiple lane detection. In Proceedings of the IEEE International Conference on Computer Vision, pages 2921–2930, 2019.
  - Gen-lanenet: A generalized and scalable approach for 3d lane detection. arXiv, pages arXiv–2003, 2020.
  - Hierarchical recurrent attention networks for structured online maps. In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition, pages 3417–3426, 2018.

### 2 Related Work

#### 2.1 Online Mapping: 

- 特点：
  - satellite imagery (卫星图像)
  - gather dense information (采集车多次经过同一地方)
  - human-in-the-loop
- predicting map elements online: 
  - 3d-lanenet: End-to-end 3d multiple lane detection. In Proceedings of the IEEE International Conference on Computer Vision, pages 2921–2930, 2019.
  - Gen-lanenet: A generalized and scalable approach for 3d lane detection. arXiv, pages arXiv–2003, 2020.

#### 2.2 Perception and Prediction

- **生成轨迹集合** generate a fixed set of trajectories [6, 8–10, 26, 28, 30, 36, 56]
- **画出样本特征分布** draw samples to characterize the distribution  
  - Implicit latent variable model for scene-consistent motion forecasting. arXiv preprint arXiv:2007.12036, 2020.
  - R2p2: A reparameterized pushforward policy for diverse, precise generative path forecasting. In ECCV, 2018.
  - Multiple futures prediction. In Advances in Neural Information Processing Systems, pages 15398–15408, 2019.

- **预测时间占用图** predict temporal occupancy maps 
  - Discrete residual flow for probabilistic pedestrian behavior prediction. arXiv preprint arXiv:1910.08041, 2019.
  - The garden of forking paths: Towards multi-future trajectory prediction. In Proceedings of the IEEE/CVF
    Conference on Computer Vision and Pattern Recognition, pages 10508–10518, 2020.
  - Scene compliant trajectory forecast with agent-centric spatio-temporal grids. IEEE RA-L, 5(2):2816–2823, 2020.
- 这些方法由于涉及了非最大抑制（non-maximum suppression）和可信度阈值（confidence thresholding），可能出现不安全的情况
- occupancy grids:
  - Motionnet: Joint perception and motion prediction for autonomous driving based on bird’s eye view maps. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pages 11385–11395, 2020.
  - Learning occupancy grid maps with forward sensor models. Autonomous robots, 15(2):111–127, 2003.
  - **Perceive, predict, and plan: Safe motion planning through interpretable semantic representations.** In Proceedings of the European Conference on Computer Vision (ECCV), 2020.

#### 2.3 Motion Planning

- 从感知直接输出控制信号 （Driving policy transfer via modularity and abstraction. arXiv preprint arXiv:1804.09364, 2018.）会面临稳定性和鲁棒性的问题（stability and robustness issues）（Exploring the limitations of behavior cloning for autonomous driving. In Proceedings of the IEEE International Conference on Computer Vision, pages 9329–9338, 2019.）

### 3 Interpretable Mapless Driving

![MP3 predicts probabilistic scene representations that are leveraged in motion planning as interpretable cost functions](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220826-2.png)

#### 3.1 Extracting Geometric and Semantic Features

- The result is a 3D tensor of size $$(\frac{H}{a},\frac{W}{a},\frac{Z}{a}\cdot T_p )$$,which is the input to our backbone network.
- This network combines ideas from [9, 53] to extract geometric, semantic and motion information about the scene.

#### 3.2 Interpretable Scene Representations 

- 动态目标的位置、速度信息，使用 **dynamic occupancy field **表示（the dynamic objects position and velocity into the future, captured in our dynamic occupancy field）

























### 总结

