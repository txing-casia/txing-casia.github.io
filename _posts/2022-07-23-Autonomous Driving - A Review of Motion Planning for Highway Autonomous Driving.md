---
layout:     post
title:      "Autonomous Driving | A Review of Motion Planning for Highway Autonomous Driving  "
subtitle:   ""
date:       2022-07-23
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## A Review of Motion Planning for Highway Autonomous Driving  

- 高速路具有路径高速（high speed）、路径曲率小（small curvature roads）、规则具体（specific driver rules）的几项特点。
- 主要面临的问题：变道（Lane change），避障（obstacle avoidance），跟车（car following）合并道路（merging）  

### 1 Introduction

- automakers now try to personalize Advanced Driving Assistance Systems (ADAS) to the driver’s style [7]. 
  -  [7]. M. Hasenjager and H. Wersing, “Personalization in advanced driver "assistance systems and autonomous vehicles: A review," in IEEE Int. Conf. on Intelligent Transportation Systems (ITSC), 2017.  

- 现有的一些辅助技术：
  - 巡航控制中的纵向合/横向舒适度和安全性（longitudinal and lateral comfort and security with the Cruise Control (CC)）
  - 智能速度自适应（Intelligent Speed Adaptation (ISA)）
  - 道路保持辅助（Lane Keeping Assist (LKA)）
  - 离道警报（Lane Departure Warning (LDW)）
- 自动驾驶分类标准：（With such an evolution in the automotive field, the Society of Automotive Engineers (SAE) published a standard classification for autonomous vehicles with a 6-level system, from 0 (no control but active safety systems) to 5 (no human intervention for driving) [10]）
  - SAE International J3016, accessed 2018-11-03. [Online]. Available: https://www.sae.org/standards/content/j3016 201401/  
- 2007 DARPA城市挑战赛：since the Defense Advanced Research Projects Agency (DARPA) organized autonomous vehicle competitions in 2004, 2005, and 2007, and thanks to new technologies, autonomous functions are evolving quickly and treat more complex scenarios in real environments. The 11 finalist teams of the DARPA Urban Challenge 2007 [11] succeeded in navigating through a city environment.   

- Furthermore, highways seem to be the first environment where drivers would be confident driving in a fully autonomous mode [26]  

### 2 Considerations for highway motion planning 

#### A. Terminology

- **ego vehicle**表示被掌控（mastered）并装备传感器（sensors-equipped）的车辆

- **obstacles vehicle**表示其它车辆，都被视为障碍物。
- 车辆的states包括：
  - position
  - orientation
  - and their 
  - time derivatives (position, speed, and acceleration, linear and angular)  

- The **geometric state space** is called the **configuration space**  

- The **evolution space** identifies the **configuration space-time** in which the ego vehicle can navigate.  

- configuration 和 evolution 空间被分为三个子空间：
  - the **collision space**, in which the ego vehicle collides with obstacles; 
  - the **uncertain space**, in which there exists a probability for the ego vehicle to be in collision;
  - the **free space**, in which there is no collision.   
  - **free-space** (spatial geometric zones)
  - **path** (sequence of spacerelated states in the free space, i.e. geometric waypoints)
  - **trajectory** (sequence of spatiotemporal states in the free space, i.e. time-varying waypoints)  
  - **maneuver** (predefined motion, considered as a subspace of paths or trajectories, i.e. motion primitives)  
  - **generation**, which builds sequences of paths, trajectories, maneuvers, or actions  
  - **planning**, meaning the selection of one sequence among the generated motions  
  - **prediction horizon** denotes the space or/and time horizon limit for the simulation of motion  

#### B. Motion Planning Scheme  

- 分层的自动驾驶算法框架

![A hierarchical scheme of Autonomous Ground Vehicle systems.](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/20220731-1.png)

- 行为规划包括：(i) route planning, (ii) prediction, (iii) decision making, (iv) generation, and (v) deformation.  

![Motion planning functions. Motion planning acts as a global, local, and reactive motion strategy.](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/20220731-2.png)

其中，decision making, generation, and deformation是核心。参考[32][33]两篇文章，总结的方法如下：

- **A high-level predictive planning** built around three objectives: risk evaluation, criteria minimization, and constraint ubmission (see II-D). Those are used for decision making (iii), i.e. to select the best solution out of the candidates’ generation (iv). One either generates a set of motions and then makes a decision on the behavior motion, or, defines the behavior to adopt and then fits a set of motions. This high-level stage benefits from a longer predicted motion but is time-consuming.  

- **A low-level reactive planning** deforming the generated motion from the high-level planning according to a reactive approach, i.e. the deformation function (v). This acts on a shorter range of actions and thus has faster computation.  


[32]. L. Claussmann, A. Carvalho, and G. Schildbach, “A path planner for autonomous driving on highways using a human mimicry approach with binary decision diagrams,” in IEEE European Control Conference (ECC), 2015.
[33]. X. Li, Z. Sun, Q. Zhu, and D. Liu, “A unified approach to local trajectory planning and control for autonomous driving along a reference path,” in IEEE Int. Conf. on Mechatronics and Automation (ICMA), 2014.  

- 空间和时间约束：

![Motion planning functions. Motion planning acts as a global, local, and reactive motion strategy.](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/20220731-3.png)

#### C. Specificities of Highway Driving  

- 特点：
  - 单向车流
  - 高速a dynamic speed over 60km/h
  - 道路形状简单：直线道路（straight lines），回旋曲线道路（clothoids），小曲率的环形道路（circles with small curvature）
- 障碍车的行为预测分为以下几个：
  - one-direction
  - two-lane changes – right or left
  - and to accelerate, maintain speed, or brake  

- 高速路的一些通常境况：

  - **Lane keeping**  

    纵向安全的情况下保持期望的速度行驶

  - **Car following**  

    跟随自己前方的车辆，保持安全距离

  - **Lane changing**  

    受到方向和障碍物的约束，规划需要保证目标车道由充足的空间和合适的行驶速度

  - **Lateral-most lane changing**  

    一些情况下的交规要求只能在最左/右的车道行驶，因此agent会一直寻求变道的机会，直到到达目标车道

  - **Passing**  

    在侧向有障碍物的时候遵守lane keeping或者car following决策的情况，需要保证侧向的安全距离  

  - **Overtaking**  

    超车上复杂的机动动作，包括变道、pass、变道三个过程

  - **Merging**  

    两个车道合并为一个车道

  - **Highway toll**  

    高速收费站，先并入虚拟的车道线，进入收费站，之后再加速驶出，并入实际的车道线

  - 高速场景特点总结：The main differences between highway, except for platooning, and city driving consist in a further look-ahead time, with a stronger focus towards the ahead direction of the road, whereas city driving involves a closer range but in all directions. The highway vehicle dynamics is also simpler with lower turn-angle, no reverse, and less braking/acceleration, but higher and more constant speed. Thus, even if there are less hazards, the risk due to high speed is stronger. Moreover, the higher distances imply poorer sensors capacities. Finally, less traffic insures more stable scenario. The algorithms which consider all these specificities in real-time will be favored for a practical application on highways.  

  - [34]. L. Claussmann, M. Revilloud, S. Glaser, and D. Gruyer, “A study on ai-based approaches for high-level decision making in highway autonomous driving,” in IEEE. Int. Conf. on Systems, Man, and Cybernetics (SMC), 2017.  

#### D. Constraints on Highway Driving  

- **硬约束**（hard constraints）：环境约束、交规、安全约束、避免碰撞。

- **软约束**（soft constraints）：时间/距离/能耗最小化，舒适性最大化等乘坐优化约束。





### 总结





​	
