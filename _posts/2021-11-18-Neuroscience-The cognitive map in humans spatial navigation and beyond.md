---
layout:     post
title:      "Neuroscience | The cognitive map in humans spatial navigation and beyond"
subtitle:   ""
date:       2020-11-18
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Neuroscience
---

# The cognitive map in humans: spatial navigation and beyond

论文链接：https://www.nature.com/articles/nn.4656#Sec1

## 背景

- 认知地图通过海马中的place cells, grid cells, border cells  and head direction cells实现during spatial navigation  

  

## 主要工作

- **spatial coding**, **landmark anchoring** and **route planning**:

  i) 人海马和内嗅皮质支持类似地图的空间代码，

  ii) 后脑区域（如海马旁和脾后皮质）提供了重要的输入，使认知图可以固定在固定的环境地标上，

  iii) 将海马和内在空间代码与额叶机制结合使用，以计划航行期间的路线。

- The idea of a cognitive map was originally proposed by Tolman, in an effort to explain navigational behaviors in rodents that could not be logically reduced to associations between specific stimuli and
  rewarded behavioral responses.
- 快速切换a roundabout route到直接路径  
- Grieves, R.M. & Jeffery, K.J. The representation of space in the brain.*Behav. Processes* 135, 113–131 (2017).这篇文章支持多种细胞共同构建了认知地图：
  - (i) grid cells in medial entorhinal cortex, which fire in a regular hexagonal lattice of locations tiling the floor of the environment; 
  - (ii) head direction (HD) cells in several cortical and subcortical structures, which fire on the basis of the orientation of the head in the navigational plane; and 
  - (iii) border cells in entorhinal cortex and boundary cells in subiculum, which fire when the animal is at set distances from navigational boundaries at specific directions.
- 标定认知地图需要海马、内侧前额叶的参与。导航过程中，可以完全使用路标来进行导航，无需路径集成，这种导航称为landmark-based piloting. Gallistel, C.R. *The Organization of Learning* (MIT Press, 1990).

- 基于路标的方法：在途中设计decision points，点之间用欧氏距离实现最短路径
- 到达同一个目标位置可以有多个路径，这就涉及到了路径规划，与前海马和place cells有关。Wu, X. & Foster, D.J. Hippocampal replay captures the unique topological structure of a novel environment. *J. Neurosci.* **34**, 6459–6469 (2014).
- 两篇强化学习相关的路径规划文章，这可能证明解剖神经系统的支持路径规划的有效途径。分层规划涉及背顶额叶区域，与目标的距离无关。在D所示的示例中，分层计划可用于将环境的各个部分组合在一起以减少计划需求：
  - Simon, D.A. & Daw, N.D. Neural correlates of forward planning in a spatial decision task in humans. *J. Neurosci.* **31**, 5526–5539 (2011).
  - Ribas-Fernandes, J.J. et al. A neural signature of hierarchical reinforcement learning. *Neuron* **71**, 370–379 (2011).



## 总结

本文介绍了很多认知地图相关的研究，重点从空间编码、地标标定和路径规划三个方向展开调研。

- 在空间编码上使用了grid cells
- 地标确定和路径规划上使用了place cells（PPA，Parahippocampal Place Area）