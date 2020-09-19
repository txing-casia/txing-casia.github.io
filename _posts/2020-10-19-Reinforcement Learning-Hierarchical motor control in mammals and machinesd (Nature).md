---
layout:     post
title:      "Reinforcement Learning | Hierarchical motor control in mammals and machinesd (Nature)"
subtitle:   ""
date:       2020-09-19
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
    - HRL
---

# Hierarchical motor control in mammals and machines 

论文链接：https://www.nature.com/articles/s41467-019-13239-6

## 背景

很多人工智能领域的进展启发了神经科学的研究。多数神经科学的研究都致力于探索单个任务空间的离散任务，而AI研究主要在agent玩复杂的游戏上。近期生物上的“synthetic motor control”却很少有人讨论。

本文是一篇讨论AI和神经科学关于HRL以及H control的综述，角度比较好，介绍了大量细分方向的工作，非常值得一读。

## 主要工作

- 神经科学启发了机器人中的subsumption architecture，简单搜了一下类似于层级机构的雏形（1986）。
- Optimal feedback control (OFC)研究如何优化运动，降低优化函数的值，取得最优性能；神经科学研究的是如何理解通过神经系统生成不同的行为动作。

下面介绍几个重叠领域的重要工作：

### 1. computational approaches to motor control

- 运动控制在animal and artificial system中都是值得关注的问题；
- 控制通过一个controller或者policy
- 图1提供了三种控制结构，

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20200917-1.png)

- 生物：内部前向模型Internal forward models are used to predict the future consequences of actions。该模型是允许虚拟行为的，可以预测行为后果。

- OFC被用于解释volitional control意志控制

  - Diedrichsen, J., Shadmehr, R. & Ivry, R. B. The coordination of movement: optimal feedback control and beyond. Trends Cogn. Sci. 14, 31–39 (2010).
  - Scott, S. H. The computational and neural basis of voluntary motor control and planning. Trends Cogn, Sci, 16, 541–549 (2012).  

- OFC几乎可以概括所有的闭环控制算法，一些扩展的OFC主要是考虑了以下三点：

  - 运动控制目标是最大化一个优化函数；
  - 利用传感反馈来进行行为选择；

  - 内部模型帮助弥补传感延迟和执行状态估计；

- OFC对目标函数的组成结构非常包容，甚至不需要一个直接的目标，但在快速求解上有一点缺陷

### 2. Motor control of synthetic systems  

- 这部分内容主要涉及DRL，没什么值得说的。
- planning methods: 
  - Mordatch, I., Todorov, E. & Popović, Z. Discovery of complex behaviors through contact-invariant optimization. ACM T, Graphics (TOG) 31, 43 (2012).
  - Mordatch, I., Wang, J. M., Todorov, E. & Koltun, V. Animating human lower limbs using contact-invariant optimization. ACM T. Graphic. 32, 203 (2013)  

- 导航任务：Jaderberg, M. et al. Reinforcement learning with unsupervised auxiliary tasks. In International Conference on Learning Representations, 2017.  

- **挑战1：**DRL的优化目标比较狭窄比如走路faster，保持平衡稳定，运动到某个位置等等，很难设计有泛化能力的目标。
- **挑战2：** **处理变化的目标**。通过其他任务的经验生成运动，适应变化的目标。

### 3. Core principles of hierarchical motor control  

#### 3.1 Information factorization

- Information factorization （信息因子分解）：分层系统提供局部或者预处理的信息给其它系统的过程
- 运动中有一部分模式的通用的，要么high-level behavior是invariant ，要么low-level behavior是invariant
- That some goals or tasks can be solved by a multiplicity of execution details (“motor equivalence”) has long been recognized as important in movement science  and has also been identified as relevant for robot control. 有些任务可以用等效的运动去完成，这也是机器人控制上的一个问题

#### 3.2 Partial autonomy  

- 完整的优化一个动作成本很高，但是只要产生出了一次动作，可以训练一个“reactive” subsystem重复地长生这个运动而不用冗余的规划过程。这个过程涉及到局部自主（partial autonomy）以及半自主系统（semi-autonomous subsystem），在算法层面主要是加载之前算出来的解。
- self-supervised learning ，生成的轨迹永续训练另外的系统in an amortized fashion（用分期的方式）

#### 3.3 Modular objectives  

- 很多ANN在用于控制问题时使用end2end的优化，这样就只能设置一个优化目标，并最大化它
- 使用模块化的子系统需要制定独特的模块化子目标
- 一种流行的方法是训练控制器解决任务的同事也训练一组内部状态表征（internal state representations）来预测未来的传感数据，某种程度上来讲，这也是一种分层的控制系统。
- 另一个景点的方法同样保持了层级的目标结构，divideand-conquer strategy。例如高层控制器决定运动的方向，低层控制器去实施，高层控制器的信号也可以作为 a dense teaching signal that the low-level controller learns from as it assesses how well it stays on the instructed course。高层口农资器处理更全局的任务。代表工作：（前两篇低层系统优化目标固定，最有一批FuN是学习抽象目标空间）
  - Nachum, O., Gu, S., Lee, H. & Levine, S. **Near-optimal representation learning for hierarchical reinforcement learning**. In International Conference on Learning Representations, 2019.  
  - Wayne, G. & Abbott, L. F. Hierarchical control using networks trained with higher-level forward models. Neural Comput. 26, 2163–2193 (2014)  
  - Vezhnevets, A. S. et al. Feudal networks for hierarchical reinforcement learning. In Proceedings of the 34th International Conference on  achine Learning, 3540–3549. JMLR. org, 2017.  

#### 3.4 Multi-joint coordination  

- motor synergy concept （Synergies in coordination: a comprehensive overview of neural, computational, and behavioral approaches. J. Neurophysiol. 120, 2761–2774 (2018).  ）

- 在肌肉驱动的系统上，从高层面设计任务显然会更容易，例如reaching and grasping. This is perhaps most readily apparent in a setting like reaching and grasping, where random movement of all degrees of freedom independently will be ineffective, but random movements in the subspace of hand configurations encountered during grasping will lead to more effective interactions.  

- 几个机器人学方面的工作: 
- Vukobratović, M. & Borovac, B. Zero-moment point—thirty five years of its life. Int. J. Hum. Robot. 1, 157–173 (2004).
- Todorov, E., Li, W. & Pan, X. From task parameters to motor synergies: a hierarchical framework for approximately optimal control of redundant manipulators. J. Robot. Syst. 22, 691–710 (2005).
- Mordatch, I., De Lasa, M. & Hertzmann, A. Robust physics-based locomotion using low-dimensional planning. ACM T. Graphic. 29, 71 (2010).  

- unsupervised learning (or self-supervised learning)  of  sensorimotor primitives in order to produce a learned low-level controller  
  - Todorov, E. **Optimality principles in sensorimotor control**. Nat. Neurosci. 7, 907 (2004).  
  - Todorov E. & Ghahramani., Z. Unsupervised learning of sensory-motor primitives. In Proceedings of the 25th Annual International Conference of the IEEE Engineering in Medicine and Biology Society, Vol. 2, 1750–1753. IEEE, 2003.  

#### 3.5 Temporal abstraction  

- 通过设置时间间隔简化使得高层控制器更宏观，但会损失控制精度，这需要低层的控制器去完成。
- 对于机器人操作任务，离散情况下只有时间维度是最好抽象的，在连续控制的问题上，多关节的协同比时间抽象更有决定性的作用。当然长期的行动规划和行为选择是需要时间抽象（Temporal abstraction）的。

### 4. Neurobiological hierarchical motor control  

- 这部分主要就少神经科学上关于分层控制的研究工作。
- 神经系统在结构和功能和功能上都有分层（both anatomically and functionally hierarchical）  

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20200918-1.png)

#### 4.1 “Lower-level” movement centers

- 脊髓（spine）在没有高层输入的情况下也可以控制躯体运动；脊髓环路能生成多关节的时空协同模式 (spatiotemporal coordination patterns)，通过中心模式生成器（central pattern generators
  (CPGs) ）产生虚构的动作（ “fictive” locomotion）；也可以通过传感信息局部调制关节活动。（CPG）
- 多关节协同通常需要：cerebellum-derived signals，somatic feedback（躯体反馈）以及inputs from other sensory systemts（传感输入）
- CPG主要产生节律的运动，比如走路，鱼摆尾等等

#### 4.2 Subcortical “mid-level” movement regulation  

- 1）皮层对于运动来说并不是必要的。因为去除了皮层的cat在恢复一段时间后，可以具备避障能力（刚切除时没有）。这反映出层次化控制和局部自治。
- 2）皮层下的结构可以选择运动协调模式
- 3）没了皮层后，传感受到损伤。但还可以通过non-cortical的通路接收传感信息。这里体现了信息因子分解。
- 4）小脑负责运动协调，基底神经节负责行为选择；

#### 4.3 Cortical “high-level” control of movement  

- 具备一些分层结构特征

### 5. Shared challenges for biological and synthetic motor control  

- despite significant progress in artificial intelligence research over the past years, there remain meaningful challenges
  in dealing with rich sensation, a broader range of tasks, rapid adaptation or improvisation, as well as object interaction and tool use.
  - 处理多传感融合
  - 处理更普遍的任务
  - 快速适应、即兴创作
  - 物体交互
  - 使用工具

#### 5.1 Towards full-scale body control  

- 对生物力学、肌肉系统、肌肉骨骼系统的研究
- 针对这类系统有两个控制思路：
  - 训练该系统解决不同的任务
  - 对观测到的行为生成数据驱动的模型

#### 5.2 The structure of inter-region communication  

- 脑区之间的编码方案(coding schemes)仍然未知，我们同样不确定分层控制系统中的信息流的形式。
- 三个问题：
  - 一个问题是分层的学习系统层级间的通信不一定需要有明确的可解释性，除非可解释的结构能带来明确的好处
  - 一个系统的输出如何调制另一个系统，对目标的调制应该是线性的还是非线性的？
  - 关于分辨率问题，表示具体行为的抽象目标之间的通讯和对下级系统丰富的指令怎么保持平衡

- 生物的角度来看，层次化的系统间一定要有一种语言或者编码，这种上下层级之间的通讯方案是AI和神经科学应该共同讨论的问题。

#### 5.3 Ethological （行为学的） motor learning and imitation  

- self-directed rehearsal: songbird在雏鸟阶段学习唱歌
- 主动采样策略：Gottlieb J. & Oudeyer, P. Y. **Towards a neuroscience of active sampling and curiosity**. Nat. Rev. Neurosci. 19, 758–770 (2018)  


## 总结

这篇综述很好地概括并梳理了在AI和生物神经科学中的零散的工作，总结出了很多子领域和本质问题，这是一篇很好的综述，可以帮助对领域的快速了解。







