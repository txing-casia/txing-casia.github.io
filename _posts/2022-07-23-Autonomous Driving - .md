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









### 总结





​	
