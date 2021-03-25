---
layout:     post
title:      "Neuroscience | A Gain-Field Encoding of Limb Position and Velocity in the Internal Model of Arm Dynamics"
subtitle:   "运动基元"
date:       2021-03-25
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Neuroscience
    - Motor Primitives
---

# A Gain-Field Encoding of Limb Position and Velocity in the Internal Model of Arm Dynamics

论文链接：https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025

## 背景

reaching任务的适应性取决于大脑中的计算，这种计算将手臂的位置和速度的感觉线索转换为运动指令。

本文提出一种位置-速度增益场来进行这种转换。

增益场中的运动基元由线性的位置编码基元和高斯形的速度编码基元乘法调制。

文章得出两个结论：

1. 在被训练的空间之外，增益场依然能帮助泛化

2. 非单调的力模式（nonmonotonic force patterns）比单调的力模式（monotonic force patterns）更难学习

## 主要工作

- 行为（[Shadmehr和Mussa-Ivaldi，1994](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Shadmehr2)；[Conditt和Mussa-Ivaldi，1999](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Conditt1)）和神经生理学（[Li等，2001](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Li1)；[Gribble和Scott，2002）](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Gribble1)）证据表明，大脑通过高度适应性强的内部模型控制肢体运动，

- 神经生理学实验表明运动皮层可能是学习肢体动力学内部模型的神经系统的重要组成部分之一（[Li et al. 2001](https://www.cell.com/neuron/fulltext/S0896-6273(01)00301-4)）。现在有关于各种运动参数如何变化的重要信息，例如肢体速度（[Moran和Schwartz，1999年](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Moran1)），手臂方向（[Scott和Kalaska，1997年](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Scott1)；[Scott等，1997年](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Scott2)）以及手部姿势（[Caminiti等，1990年](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Caminiti1)；[Sergio和Kalaska](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Sergio1)，[1997年](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Scott2)）[1997](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Sergio1)）由运动皮层中的神经元编码。

- 具有反映在神经生理学实验中发现的某些细胞特性的元素的计算模型已尝试解释适应过程中泛化的模式如何与神经表征相关。这些计算模型假设内部模型由“元素”或碱基组成，每个仅编码感觉空间的一部分，并且人口代码在计算感觉运动转换时会结合这些元素（[Georgopoulos等人1986](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Georgopoulos2)；[Levi和Camhi 2000](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Levi1)；[Pouget等人2000](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Pouget2)；[Thoroughman和Shadmehr 2000](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Thoroughman1)；[Donchin和Shadmehr 2002](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Donchin1)；[Steinberg等人2002。](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Steinberg1)）。该图将期望的感觉状态转换为即将到来的力的预测。在这些假设下，错误概括的模式应揭示基本元素的形状。

- 我们进行了一组实验，检查了神经元如何同时编码肢体位置和速度。我们表明，运动误差一般用一种模式暗示，该模式表明肢体位置空间的线性或单调编码，并且该编码是通过运动方向的编码进行乘法调制的。我们从泛化模式中得出的肢体位置和速度的增益场编码与运动皮层中这些参数的神经编码非常相似（[Georgopoulos等，1984](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Georgopoulos1)）

- 实验部分：

  - 每个运动的关节转动角度相同，运动时间相同，因此保证探索相同的关节速度-位置空间
  - 力场为 **viscous curl-field**（粘性卷曲力场）
  - **catch trial** 是不加力场的运动；**field trial** 是加力场的运动
  - 左边的轨迹，力场向左推手；
  - 右边的轨迹，力场向右推手

  - 中间的轨迹，力场输出为0

- 运动相隔越远，力越能联系它们

- 以上结果表明，当不同的力与相同方向但在不同空间位置的两个运动相关联时，泛化会随着它们之间距离的增加而降低。另一方面，较早的结果发现，当在单个位置学习到不同方向的运动时，学习会推广到很远的其他手臂位置（[Ghez等，2000](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Ghez1)；[Shadmehr和Moussavi，2000](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Shadmehr1)；[Malfait等，2002](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Malfait1)）。

  The above results demonstrate that when different forces are to be associated with two movements that are in the same direction but at different spatial locations, generalization decreases with increased distance between them. On the other hand, earlier results had found that when movements to various directions are learned at a single location, learning generalizes to other arm locations very far away ([Ghez et al. 2000](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Ghez1); [Shadmehr and Moussavi 2000](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Shadmehr1); [Malfait et al. 2002](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Malfait1)).

- 我们同时考虑了位置空间的高斯编码和线性编码。我们首先假设肢体位置空间为高斯编码，并评估基础元素的最佳宽度以适合数据。我们惊讶地发现，每个元素的半峰处最佳全宽约为80厘米（高斯函数的标准偏差，σ= 34厘米）。通过高斯基础元素对位置空间进行非常广泛的调整，在工作空间宽度的三倍的工作空间上形成了一个基本上线性的位置相关的接收场。由于此模型在我们的整个训练空间以及以后的范围内都产生了基本上单调的位置编码，因此我们决定详细研究具有简单线性位置编码的模型。的确，**运动皮层**的研究（[Georgopoulos等人，1984年](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Georgopoulos1)；[Sergio和Kalaska（1997](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Sergio1)），**体感皮质**（[Prud'homme和Kalaska 1994](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Prudahomme1) ; [Tillery等人1996](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Tillery1)）和**脊髓小脑束**（[Bosco等人1996](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Bosco1)）发现，这些区域的细胞在全局范围内通常是**线性地编码肢体静态位置**。

- **加性编码不能适应作为位置和速度的非线性函数的场**，例如*f*（*x*，*ẋ*）=（*x / d*）· *Bẋ*。这是描述我们在上一节中考虑的任务的力场。理论研究表明，要适应这样的非线性场，必须以乘法而不是加法形成组合空间的基函数（[Pouget和Sejnowski 1997](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Pouget1)）。我们选择使用位置和速度的乘法组合。

- **假设位置和速度通过增益场机制联合编码。**We hypothesized that position and velocity encoding are combined via a gain-field mechanism 

- 图3B是两个基元，线性力场的情况下的基元权重学习过程，曲线越与中间斜率为-1的虚线对齐，表示学习越快越高效。（1/2kd,-1/2kd）是最佳权重。k越大、d越大、b越小，越能出尽快速学习。

  - $$
    f(x)=\frac{x}{d}\\
    g_1=kx+b\\
    g_2=-kx+b
    $$

- Importantly, when the trained position *d* is close to the zero of the coordinate axis, the slope is also close to zero, making the generalization function flat (i.e., global generalization).

- 适应力场并不只是单纯地增加刚度。This was not simply a result of **increased stiffness** because, when the force field was removed unexpectedly, the trajectories and endpoint errors were on the opposite side of those in the early force field, indicating **a proper internal model**. [Figure 4](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio-0000025-g004)C shows a position-dependent field in this category, and [Figure 4](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio-0000025-g004)D–[4](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio-0000025-g004)F shows movements made by the simulation as it adapts to this field. This pattern of adaptation is similar to reported values in human data ([Flash and Gurevich 1997](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.0000025#pbio.0000025-Flash1)).

- 图4AB，在L区域训练，然后再R区域测试，在新力场中学习速度变快

- 两个模型的预测：

  (1) a change to the pattern of forces can substantially increase the difficulty of a task; 力场中变化的力模式会大大增加任务难度

  (2) there should be hypergeneralization; i.e., forces expected in an untrained part of the workspace may be larger than the ones experienced at a trained location.超泛化性，在工作空间的未训练部分预期的力可能比在训练位置经历的力大

- 尽管在训练空间上学得很少，但我们在测试空间中发现了明显更大的后效应，即过度概括。我们的模拟表明，这种过度概括不能归因于**肢体惯性**和/或**刚度**随肢体位置的变化而变化。More importantly, despite this small learning in the training space, we found significantly bigger aftereffects in the test space, i.e., hypergeneralization. Our simulations suggest that this hypergeneralization could not be due to varying limb inertia and/or stiffness as a function of limb position.

- **增益场定义**：细胞编码中两个独立变量的乘法相互作用称为增益场编码。Multiplicative interaction of two independent variables in cell encoding is called gain-field coding.



## 总结

很有趣的文章，值得进一步考虑。但是提出的模型比较简单，可以进一步讨论。

