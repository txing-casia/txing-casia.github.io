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

- 道路先验信息和一些可解释的知识，使用 `online map` 表示
- 动态目标的位置、速度信息，使用 `dynamic occupancy field` 表示（the dynamic objects position and velocity into the future, captured in our dynamic occupancy field）

![Interpretable Scene representations](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220826-3.png)

- 具体而言，两种表征信息包括：

**Online map representation:**

- Drivable area：以道路边缘为界的可行驶区域；
- Reachable lanes：可用车道是SDV在不违反任何交通规则的情况下可以到达的运动路径的子集。规划轨迹时，我们希望SDV靠近这些可到达的车道，并按照它们的方向行驶。因此，对于地平面中的每个像素，我们预测到最近的可到达车道中心线的无符号距离，在10米处截断，以及最近的可到达车道中心线分段的角度。
- Intersection：被交通信号等或者交通标志控制的路段，需要根据信号灯或者标志按交通规定行驶；

**Dynamic occupancy field:**

现有的行为预测算法包括不安全的离散决策unsafe discrete decisions such as confidence thresholding and non-maximum suppression (NMS)

![The motion field warps the occupancy over time](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220826-4.png)

- Initial Occupancy：一个BEV网格单元
- Temporal Motion Field：a 2D BEV velocity vector (in
  m/s).
- `Note`：车辆、行人和自行车被视为单独的类别，每个类别都有自己的占用流。

**Probabilistic Model:**

online Map 分为以下几个通道：

- 可到达区域$$M^A_i$$
- 路口$$M^I_i$$

- 到最近车道线的距离。the direction of the closest lane centerline in the reachable lanes $$M^{\theta}_i$$ as a Von Mises distribution since it has support between $$[\pi,\pi]$$.
- 可到达车道中线的截断距离变换为拉普拉斯算子。We model the truncated distance transform to the reachable lanes centerline $$M^D_i$$​ as a Laplacian, which we empirically found to yield more accurate results than a Gaussian

建模动态物体的occupancy $$O^c$$,为伯努利随机分布$$O^c_{t,i}$$，$$c\in \{行人，车辆，自行车\}$$（考虑这些物体未来行为的多模态（直走或左转）和不确定性），用$$K_{t,i}$$建模基于K个BEV运动向量$$V^c_{t,i,k}$$的行为分类分布

the probability of future occupancy under our probabilistic model, we first define the probability of occupancy flowing from location $$i_1$$ to location $$i_2$$ between two consecutive time steps t and t + 1 as follows:

![the probability of occupancy flowing](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220829-1.png)

#### 3.3 Motion Planning

设计了一个基于采样的轨迹规划器，其根据运动学灵活的生成多种轨迹，然后使用一个learned scoring function选择轨迹。

![规划器的轨迹选择方式](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220830-1.png)

















### 总结

