---
layout:     post
title:      "Autonomous Driving | A Review of Motion Planning for Highway Autonomous Driving"
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

![A hierarchical scheme of Autonomous Ground Vehicle systems.](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220731-1.png)

- 行为规划包括：(i) route planning, (ii) prediction, (iii) decision making, (iv) generation, and (v) deformation.  

![Motion planning functions. Motion planning acts as a global, local, and reactive motion strategy.](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220731-2.png)

其中，decision making, generation, and deformation是核心。参考[32][33]两篇文章，总结的方法如下：

- **A high-level predictive planning** built around three objectives: risk evaluation, criteria minimization, and constraint ubmission (see II-D). Those are used for decision making (iii), i.e. to select the best solution out of the candidates’ generation (iv). One either generates a set of motions and then makes a decision on the behavior motion, or, defines the behavior to adopt and then fits a set of motions. This high-level stage benefits from a longer predicted motion but is time-consuming.  

- **A low-level reactive planning** deforming the generated motion from the high-level planning according to a reactive approach, i.e. the deformation function (v). This acts on a shorter range of actions and thus has faster computation.  


[32]. L. Claussmann, A. Carvalho, and G. Schildbach, “A path planner for autonomous driving on highways using a human mimicry approach with binary decision diagrams,” in IEEE European Control Conference (ECC), 2015.
[33]. X. Li, Z. Sun, Q. Zhu, and D. Liu, “A unified approach to local trajectory planning and control for autonomous driving along a reference path,” in IEEE Int. Conf. on Mechatronics and Automation (ICMA), 2014.  

- 空间和时间约束：

![Motion planning functions. Motion planning acts as a global, local, and reactive motion strategy.](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220731-3.png)

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

- 其他可行性约束依赖于车辆的运动学限制，即非完整动力学，即车辆在只有两个自由度的三维空间中发展，平滑路径，即轨迹应该可微分且其曲率连续，以及车辆的动力学限制。

- 作者在文献[27]中认为，生成运动的质量要求应该为：可行、安全、最优、可用、自适应、高效、渐进和交互（feasible, safe, optimal, usable, adaptive, efficient, progressive, and interactive）
- [27]. M. Rodrigues, A. McGordon, G. Gest, and J. Marco, “Adaptive tactical behaviour planner for autonomous ground vehicle,” in IEEE Int. Conf. on Control, 2016.  

### 3 State of the art

-  运动规划包含了5个方面：
  - (i) state estimation
  - (ii) time evolution
  - (iii) actions planning
  - (iv) criteria optimization
  - (v) compliance with constraints  

- 一个争论：是否要区分驾驶模式（distinguishing and not distinguishing the
  driving modes），不同模式下使用不同的数据进行学习。

- **分类1：空间构造算法**（Space Configuration）

  - 总述：sampling points, connected cells, and lattice这些方法的核心思想在于：（1）对进化空间进行采样或离散化；（2）排除与障碍物冲突或不可行的点、单元或网格；（3）将这些空间分解作为自由空间约束发送，或者用寻路算法(见III-B2)或曲线规划器(见III-B4)求解结果空间配置，以直接将路点、连接单元集或点阵集发送到控制块。

  - Sampling-Based Decomposition：

    - Probabilistic RoadMap (PRM) [41]（The most popular random method），配合Dijkstra算法[42]，先选择路径点，再分配速度曲线
    - spatiotemporal sampling points predictive algorithm[43]，采样5维的车辆状态点（车的位置、速度、角度、到达时间），考虑空间分辨率的因素，还可以结合自适应分辨率采样方法[44]

  - Connected Cells Decomposition：网格化道路，赋予格子随机权重，然后避障寻路，该方法的缺点在于要求大的记忆容量、较高的计算速度、具有移动障碍物的虚假指示性占领，以及随空间和时间变化的分辨率。

    依据不同的速度和形状，障碍物通常表示为凸多边形（convex polygons） 、矩形、 三角形、圆形、椭圆形。

    - **非基于障碍物表征的方法**，格子组织可以离线觉得在线填充，网格可以快速获得但没有使用环境特性。eg：exact decomposition（正在淘汰） 
    - **基于障碍物表征的方法**，考虑动态变化的环境，建立在线的网格，更加方便计算和重规划。

  - Lattice Representation（晶格表征）构建reachability graph of maneuvers，多用于predictive planning。calculated offline for a quick
    replanning [76]. Unfortunately, their application to reactive
    planning is mostly limited due to the fixed structure.  

    经典的晶格表征算法基于maximum turn strategy [13, 76]，只变化车的角度，调整路径。引入速度后的改进方法curvature velocity method[77],[72], [78] 。方法的缺点在于需要预先定义的运动集，以及高密度的运动图。

- **分类2：路径搜索算法**（Pathfinding Algorithms）

  这类算法属于图论中的一部分，代表算法为Dijkstra and A*等，主要问题在于图的尺寸和复杂度，以及进一步的动态环境的处理上，总之这些方法不是太适应高速的环境条件。

- **分类3：吸引力和排斥力**（Attractive and Repulsive Forces）

  目的地是吸引力，障碍物产生排斥力，由此构建引力图产生规划的轨迹。其优点是方便适应动态的环境。其问题在于局部最优和震荡现象。

  Virtual Force Field (VFF) [56]  

  elastic band algorithm [102]  

- **分类4：参数化和版参数化曲线**（Parametric and Semi-parametric Curves）

  考虑（1）高速路本省就是由简单的曲线构成的；（2）预先定义的曲线几何很容易实施和测试；曲线路径和速度可以解耦考虑。这里介绍两类算法：

  - **point-free curves算法**：首先构建运动学上可行的轨迹，作为一组候选解；然后基于点的子类使用曲线来拟合一组选择的路点(采样点或单元)

    该方法也可以参考基于晶格的方法，用一些基本曲线构建可能的运动路径，形成“触手”，以加快求解的速度。但是这些简单曲线的二阶导数不连续

  - **point-based curves算法**：能很好适应约束环境的几何特征，各种曲线的选择依赖对环境的认知。

- **分类5：数值优化**（Numerical Optimization）

  数值优化方法在运动规划中被广泛使用。一类算法简化求解的复杂度，提高效率；一类算法探索数学上的性质，以在受限的空间（restrictive space）中推断出预测解。对于第二类方法，其基础的算法是the Linear Programming (LP)  （most popular one: Simplex algorithm[81]）

  具体的预测上，使用Model Predictive Control (MPC)，Dynamic Programming (DP)等  

- **分类6：人工智能方法**（Artificial Intelligence）

  需要复制并模拟司机的推断和学习能力。本文将这些方法分为两类：**cognitive/rational** and **rules/learning**，– based on [125]’s distinction between thinking and acting humanly or rationally

  ![A map of AI-based algorithms.](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220801-1.png)
  
  - 人工智能基于逻辑的方法（AI Logic-Based Approach）  
  
    依赖专家知识库和规则的专家推理系统。主要缺点在于处理循环推理和枚举所有规则
  
    - 决策树：不确定性和近似值增加了计算困难，行为必须被解释为安全合法的。
  
    - 有限状态机（Finite State  Machine (FSM)）：  只能在已知的知识范围内运行，不能在未知的环境中做生成。
  
      ![Illustration of an FSM for highway](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220808-1.png)
  
    - Bayesian networks：依赖马尔科夫模型的状态转移因果链。The authors in [50] develop a Markov
      Decision Process (MDP) on the **choice of tentacle trajectories**, and the one in [130] for a **lane-staying or -changing decision.**  
  
      - [50]. H. Mouhagir, R. Talj, V. Cherfaoui, F. Aioun, and F. Guillemard, “Integrating safety distances with trajectory planning by modifying the occupancy grid for autonomous vehicle navigation,” in IEEE Int. Conf. on Intelligent Transportation Systems (ITSC), 2016.  
      - [130]. S. Zhou, Y. Wang, M. Zheng, and M. Tomizuka, “A hierarchical planning and control framework for structured highway driving,” IFACPapersOnLine, vol. 50, no. 1, pp. 9101–9107, 2017.  
  
      POMDP：
  
      - [131] S. Ulbrich and M. Maurer, “Towards tactical lane change behavior planning for automated vehicles,” in IEEE Int. Conf. on Intelligent Transportation Systems (ITSC), 2015.
      - [132] E. Galceran, A. G. Cunningham, R. M. Eustice, and E. Olson, “Multipolicy decision-making for autonomous driving via changepoint-based behavior prediction: Theory and experiment,” Autonomous Robots, vol. 41, no. 6, pp. 1367–1382, 2017.
      - [133] N. Li, D. W. Oyler, M. Zhang, Y. Yildiz, I. Kolmanovsky, and A. R. Girard, “Game theoretic modeling of driver and vehicle interactions for verification and validation of autonomous vehicle control systems,” IEEE Trans. on Control Systems Technology, vol. 26, no. 5, pp. 1782– 1797, 2018.  
  
    - 人工智能启发式算法（AI Heuristic Algorithms）  
    
      优势在于相对快速高效，但是具有启发式算法的通病，即陷入局部最优，无法保证得到全局最优。
    
      - 基于Support Vector Machines (SVM)的个性化变道决策：
        - C. Vallon, Z. Ercan, A. Carvalho, and F. Borrelli, “A machine learning approach for personalized autonomous lane change initiation and control,” in IEEE Intelligent Vehicles Symposium (IV), 2017.  
      - 演化算法（Evolutionary methods）。在高速场景，最优解并不是必须的，演化算法的高计算效率，获得近似次优解，足以满足要求。
    
  - 人工智能近似推理（AI Approximate Reasoning） 
  
    该方法与logic approach的区别在于其知识是非boolean形式表示的。 方法的优势在于其灵活，可拓展到非确定性数据中。缺点在于缺少可追踪性和系统性的设计方法
  
    - Artificial Neural Networks (ANN)  
  
      主要可分为三类方法：supervised, unsupervised, and reinforcement learning。他们的缺点在于缺少因果解释
  
      使用卷积网络变道
  
      - E. Rehder, J. Quehl, and C. Stiller, “Driving like a human: Imitation learning for path planning using convolutional neural networks,” in IEEE Int. Conf. on Intelligent Robots and Systems (IROS) Workshops, 2017.
  
      multi-goal overtaking maneuvers[144]
  
      - D. C. K. Ngai and N. H. C. Yung, “A multiple-goal reinforcement learning method for complex vehicle overtaking maneuvers,” IEEE Trans. on Intelligent Transportation Systems, vol. 12, no. 2, pp. 509–522, 2011.  
  
      automated lane change maneuvers[145]
  
      - P. Wang, C.-Y. Chan, and A. de La Fortelle, “A reinforcement learning based approach for automated lane change maneuvers,” IEEE Intelligent Vehicles Symposium (IV), 2018.  
  
  - 人工智能类人的方法（AI Human-Like Methods）  
  
    类人的驾驶模型的完整描述，参考这篇文章
  
    - D. D. Salvucci, “Modeling driver behavior in a cognitive architecture,” Human factors, vol. 48, no. 2, pp. 362–380, 2006 
  
    可以分解为风险、任务和博弈三类：
  
    - **Risk estimators**：执行可接受风险和目标安全之间的权衡。
    - **Taxonomic models**：场景和状况细分分类。  
    - **Game theory**：把所有车辆看做agent参与博弈，缺点是假设所有人准守规则，一些文章进行了改进：
      - N. Li, D. W. Oyler, M. Zhang, Y. Yildiz, I. Kolmanovsky, and A. R. Girard, “Game theoretic modeling of driver and vehicle interactions for verification and validation of autonomous vehicle control systems,”
        IEEE Trans. on Control Systems Technology, vol. 26, no. 5, pp. 1782–1797, 2018.  
  
    人工智能类似人类的方法非常适合在高速公路场景中进行决策，由于这种环境的基本规则，司机的行为更容易预测。也易于理解并与驾驶员分享。但目前还没有统一的处理框架。 
  



### 总结

这篇文章介绍的范围太大，涵盖的研究方向和方法过多，受篇幅限制，讲的东西又很浅显，价值不大。



​	
