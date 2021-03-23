---
layout:     post
title:      "Neuroscience| Principles of sensorimotor learning"
subtitle:   "很好的运动学习中涉及的计算机制综述"
date:       2020-12-17
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Neuroscience
---

# Principles of sensorimotor learning

论文链接：https://www.nature.com/articles/nrn3112

## 背景

运动的多样性和适应性给人留下深刻的印象。这篇综述回顾了运动学习的相关研究，集中讨论了其中涉及的计算机制。

运动学习涉及三个方面：

- 涉及任务相关的传感信息、决策、策略选择、预测以及反应控制机制（predictive and reactive control mechanisms）；
- Decisions and strategies：涉及不同的学习过程，例如误差和奖励如何驱动学习；
- 运动学习记忆的神经表征，以及信度分配，新状态（situations）的生成

近年来在上述三点的计算过程理解上有了本质的进步

## 主要工作

### 1. Components of motor learning

#### 1.1 Information extraction

- Skilled performance requires the effective and efficient (有效和高效的) gathering and processing of sensory information relevant to an action.

- 眼动可由自下而上和自上而下两种方式驱动。Studies have shown that eye movements can be driven both in a bottom-up, task-independent manner based on low-level features of the visual scene$$^1$$ (for example, towards moving high-contrast objects) as well as in a top-down, task-dependent manner$$^2$$. 

- 1. 多模态的传感信息流，2. Bayesian inference中对世界的先验知识，3. 结合上述过程与身体内部模型的运动指令到期望的传感输出

#### 1.2 Decisions and strategies

- 多数的运动任务都涉及了一个序列决策过程。
- 决策变量（decision variable）的积累，支持候选的行为决策
- 有边界的飘移扩散模型。Choice accuracy and reaction time are then explained by **a bounded drift-diffusion model** in which the decision variable reaches a positive or negative bound

- 这个漂移扩散模型存在着大量的延时，但实际上收集的传感信息可用来影响运动中的想法$$^{20}$$

  [20]. Resulaj A , Kiani R , Wolpert D M , et al. Changes of mind in decision-making[J]. Nature, 2009, 461(7261):263-266.

- 人在不同的实验条件下都能趋向最优的决策，适应可变性

  Trommershäuser, J., Maloney, L. T. & Landy, M. S. Decision making, movement planning and statistical decision theory. Trends Cogn. Sci. 12, 291–297 (2008).

-  决策是趋向**规避风险**还是**冒风险**（risk averse or risk seeking），偏好奖励的方差大的还是小的$$^{23-25}$$

  [23]. Nagengast, A. J., Braun, D. A. & Wolpert, D. M. Risk-sensitive optimal feedback control accounts for sensorimotor behavior under uncertainty. PLoS Comput. Biol. 6, e1000857 (2010).

  [24]. Braun, D. A., Nagengast, A. J. & Wolpert, D. M. Risk-sensitivity in sensorimotor control. *Front. Hum. Neurosci.*5, 1 (2011).

  [25]. Nagengast, A. J., Braun, D. A. & Wolpert, D. M. Risk-sensitivity and the mean-variance trade-off: decision making in sensorimotor control. *Proc. Biol. Sci.* 278, 2325–2332 (2011).

#### 1.3 Classes of control

- 三类控制都是适应性的并且贡献于运动学习：
  - **predictive or feedforward control**, which is critical given the feedback delays in the sensorimotor system; 
  - **reactive control**, which involves the use of sensory inputs to update ongoing motor commands; 
  - **biomechanical control**, which involves modulating the compliance of the limb.

- 人在拿东西的时候有预测东西的重量（预测控制），这一点对 smooth and dexterous manipulation是必要的
- 预测控制和反应控制的亲密关系：for example, the tactile afferents. If a mismatch between predicted and actual sensory information is detected, the system can launch appropriate, task-protective corrective actions and can also update the knowledge of object weight to improve future actions. Thus, through the prediction of sensory consequences, there is an intimate relationship between predictive and reactive control mechanisms$$^{28}$$.

- 快速反应反馈回路，快速驱动运动响应，但是很难被修改（有点类似于我的底层控制的意思）。Fast reactive feedback loops, such as the monosynaptic stretch reflex, can rapidly drive motor responses but cannot be easily modified even by extended experience. 

- 最小干预原则：An important feature of the optimal feedback control model is the concept of minimum intervention.

  分割任务相关空间和任务不相关空间，不相关空间中可以体现运动的可变性

  Todorov, E. & Jordan, M. I. Optimal feedback control as a theory of motor coordination. Nature Neurosci. 5, 1226–1235 (2002)

- 生物力学控制：A third form of control can be exerted by specifying the biomechanical properties of the body and tools with which we interact.  例如手臂刚度的控制。

- 学习初期，误差比较大手臂刚度增加，同时预测模型开始学习；当误差减小了，刚度也降低

  Franklin, D. W. et al. CNS learns stable, accurate, and efficient movements using a simple algorithm. J. Neurosci. 28, 11165–11173 (2008). 

-  阻抗控制（impedance control ）结合最优控制框架

Mitrovic, D., Klanke, S., Osu, R., Kawato, M. & Vijayakumar, S. A computational model of limb impedance control based on principles of internal model uncertainty. PLoS ONE 5, e13601 (2010).


### 2. Processes of motor learning

#### 2.1 Error-based learning

-  reaching in force fields

  Shadmehr, R. & Mussa-Ivaldi, F. A. Adaptive representation of dynamics during learning of a motor task. J. Neurosci. 14, 3208–3224 (1994).

  Thoroughman, K. A. & Shadmehr, R. Learning of action through adaptive combination of motor primitives. Nature 407, 742–747 (2000).

-  A common feature across these different task domains is that the system can — and will — **learn from an error on a single trial**

- 把冗余关节空间映射到二维的光标上：

  Mosier, K. M., Scheidt, R. A., Acosta, S. & Mussa-Ivaldi, F. A. Remapping hand movements in a novel geometrical environment. J. Neurophysiol. 94, 4362–4372 (2005).

  Liu, X., Mosier, K. M., Mussa-Ivaldi, F. A., Casadio, M. & Scheidt, R. A. Reorganization of finger coordination patterns during adaptation to rotation and scaling of a newly learned sensorimotor transformation. J. Neurophysiol. 105, 454–473 (2011).

#### 2.2 Reinforcement learning

-  多关节手臂的多解问题，可以建立一个解的流形（solution manifold）

  Vetter, P., Flash, T. & Wolpert, D. M. Planning movements in a simple redundant task. Curr. Biol. 12, 488–491 (2002).

- skill learning的速度精度权衡函数：A reduction in the variability for a given movement speed can be considered the hallmark of skill learning：

  Reis, J. et al. Noninvasive cortical stimulation enhances motor skill acquisition over multiple days through an effect on consolidation. Proc. Natl Acad. Sci. USA 106, 1590–1595 (2009).

- skillful movement：熟练的运动

#### 2.3 Use-dependent learning

- Use-dependent learning指的是通过不断重复，即使没有结果信息，就能改变运动效果的现象。

### 3. Representations in motor learning

- 传感运动学习涉及了建立运动与传感变量之间的新映射。这种转换也被称为内部模型。

  Such transformations are termed internal models, as they represent features of the body or the environment, such as the way in which a hand-held racquet responds to force and torques, or the way in which prism glasses change the visuomotor alignment.

- 但是很多因素会影响这个映射，例如肌肉疲劳或者改变的目标权重。一个成功的运动表现应该是可以适应这些变化的。
- Mechanistic models使用运动基元（ motor primitives）
- Normative models讲究神经系统的最优适应性。它需要两个参数：
  1. how different factors, such as tools or levels of fatigue, influence the motor system — the so-called **generative model**. 
  2.  how these factors are likely to vary over both space and time — that is the **prior distribution**. 

#### 3.1 Motor primitives

- 对手臂运动的研究，使用运动基元表征速度和位置，对运动进行近似

  Sing, G. C., Joiner, W. M., Nanayakkara, T., Brayanov, J. B. & Smith, M. A. Primitives for motor adaptation reflect correlated neural tuning to position and velocity. Neuron 64, 575–589 (2009).

#### 3.2 Credit assignment

- 两种信度分配：contextual and temporal credit assignment

- 空间信度分配设计两只手或者其他一些特殊情况，似乎与神经科学和行为科学更加相关，这里暂时不详细参考

- 误差的时间信度分配中，有个考虑了多时间尺度的学习模型。解释reaching movement适应不同的扰动。一个快速和一个慢速的系统并行地学习

  Smith, M. A., Ghazizadeh, A. & Shadmehr, R. Interacting adaptive processes with different timescales underlie short-term motor learning. PLoS Biol. 4, e179 (2006). 

  - dual-rate model解释了重学习以及自发回复之前的运动记忆

- 多时间尺度学习系统：

  Lee, J.-Y. & Schweighofer, N. Dual adaptation supports a parallel architecture of motor memory. J. Neurosci. 29, 10396–10404 (2009).

  Kording, K. P., Tenenbaum, J. B. & Shadmehr, R. The dynamics of memory as a consequence of optimal adaptation to a changing body. Nature Neurosci. 10, 779–786 (2007).

  Huang, V. S. & Shadmehr, R. Persistence of motor memories reflects statistics of the learning event. J. Neurophysiol. 102, 931–940 (2009).

#### 3.3 Structural learning

- 通过学习工具、肢体的动力学关系，可以在其他的使用类似工具的任务中再次使用这个动力学结构。已知的结构帮助快速学习。

- 对机器人自身结构的学习，帮助在面对意外受损后的运动维持

  Bongard, J., Zykov, V. & Lipson, H. Resilient machines through continuous self-modeling. Science 314, 1118–1121 (2006).

- 结构学习，挺有意思的工作：

  Braun, D. A., Aertsen, A., Wolpert, D. M. & Mehring, C. Motor task variation induces structural learning. Curr. Biol. 19, 352–357 (2009). 

  Braun, D. A., Aertsen, A., Wolpert, D. M. & Mehring, C. Learning optimal adaptation strategies in unpredictable motor tasks. J. Neurosci. 29, 6472–6478 (2009).

  Braun, D. A., Mehring, C. & Wolpert, D. M. Structure learning in action. Behav. Brain Res. 206, 157–165 (2010).

  Braun, D. A., Waldert, S., Aertsen, A., Wolpert, D. M. & Mehring, C. Structure learning in a sensorimotor association task. PLoS ONE 5, e8973 (2010)

- 有一些通过设置力场来扰乱手臂运动促进结构学习得工作



### 4. Conclusions and future directions

- From laboratory learning to real-world learning.虽然简单的任务方便建立模型，但是和实际中的任务有巨大的差别

- From sensorimotor control to robotics and brain–machine interfaces.
- From models to neuronal implementations

## 总结

又是一篇很好的综述，其中的很多点都可以深挖一下

