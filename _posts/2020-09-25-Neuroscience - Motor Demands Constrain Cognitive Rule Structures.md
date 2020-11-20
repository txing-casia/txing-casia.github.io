---
layout:     post
title:      "Neuroscience | Motor Demands Constrain Cognitive Rule Structures"
subtitle:   ""
date:       2020-09-25
author:     "Txing" 
header-img: "img/post-bg-py.jpg"
tags:
    - Neuroscience
---

# Motor Demands Constrain Cognitive Rule Structures    

论文链接：https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004785

## 背景

运动需求影响了实施抽象规则选择的结构。

低层、高层认知之间的影响。

## 主要工作

利用贝叶斯推断做规则结构的聚类。对每个输入维度单独用一个专家判别器判断sample是否属于本规则，输出为0或者1。

最后用加权的形式综合各个专家的观点$$\pi(a)=w_C(t)\pi_C(a)+(1-w_C(t))\pi_S(a)$$（两个专家情况）

然后对权重进行更新
$$
w_C(t+1)=\frac{w_C(t)p(r_t|S_t,a_t,Color)}{w_C(t)p(r_t|S_t,a_t,Color)+(1-w_C(t))p(r_t|S_t,a_t,Shape)}
$$


后文还引入了bias，具体公式见原文。

## 总结

本文有点单薄，也只是考虑了2个输入维度的情况，有点简单了。



