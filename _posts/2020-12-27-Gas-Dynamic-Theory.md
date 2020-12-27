---	
layout:     post	
title:      『Physics』 Gas Dynamic Theory	
subtitle:   『物理学』 气体动理论    
date:       2020-12-27	   
author:     Coekjan 
header-img: img/post-bg-PHY.jpg	
catalog:    true	
mathjax:    true    
tags:	
    - Physics  
---

## 状态参量与理想气体的物态方程

### 状态参量

为描述系统的平衡状态, 需采用一些物理量来描述物体的有关特性, 这些描述状态的变量称作**状态参量**.

对于一定量的气体(质量为 $m$ , 摩尔质量为 $M$ ), 一般使用以下三个状态参量来描述其状态:
1. 气体体积 $V$ , 单位 $\rm m^3$ (立方米)
2. 气体压强 $p$ , 单位 $\rm Pa$ (帕斯卡)
3. 气体温度 $T$ , 单位 $\rm K$ (开尔文)

我们称这三个量为气体的状态参量.

### 理想气体的物态方程

描述气体的三个状态参量间存在一定的关系, 我们把反应三者关系的关系式称作**气体的物态方程**. 实验表明, 一般气体在密度不太高, 与大气压相比压强不太大(大气压 $1 \rm{atm}=1.01325\times 10^5 \rm{Pa}$ ), 与室温相比温度不太低(室温 $27\rm ^\circ C \approx 300 \rm K$)时, 遵守**玻意耳定律, 盖-吕萨克定律, 查理定律**.

注意, 对于不同气体, 这三条定律的适用范围是不同的, **不易液化**的气体(氮, 氢, 氧, 氦等)适用范围较大. 事实上, 任何情况下都遵守这三定律的气体是不存在的.

我们把实际气体抽象化, 提出**理想气体**这一概念, 认为其**无条件**地服从以上三条定律.

理想气体状态的三个参量间的关系, 就称为**理想气体的物态方程**. 从三定律, 可以得到, 质量为 $m$ , 摩尔质量为 $M$ 的理想气体处于平衡态时, 物态方程为:

$$
pV=\frac{m}{M}RT
$$

式中, $R$ 为普适气体常量, 在国际单位制下, $R = 8.31 \rm J/(mol\cdot K)$

## 理想气体的微观模型

从气体分子热运动的基本特征出发, 理想气体的微观模型可以做如下假设:
1. 气体分子的大小**远小于**气体分子间的距离, 因而**气体分子的大小可以忽略不计**.
2. 由于分子力的作用距离极短, 可以认为分子气体之间**除了碰撞的瞬间外**, 分子间的**相互作用力可以忽略不计**.
3. 分子的碰撞以及分子与容器壁的碰撞可以看作**完全弹性碰撞**, **动能无损失**.

从气体分子热运动的统计性, 还须作出一些统计性的假设:
1. 气体处于平衡态时, 气体分子在空间中的均匀分布. 由此, 气体分子的数密度$n=N/V$在空间中处处相等.
2. 气体处于平衡态时, 气体分子沿空间各个方向运动的概率相等. 由此:

$$
\begin{aligned}
    &\operatorname{Avg}(\vec v_x)=
    \operatorname{Avg}(\vec v_y)=
    \operatorname{Avg}(\vec v_z)=
    \vec 0\\
    &\operatorname{Avg}(v^2_x)=
    \operatorname{Avg}(v^2_y)=
    \operatorname{Avg}(v^2_z)=
    \frac{1}{3}\operatorname{Avg}(v^2)
\end{aligned}
$$

## 理想气体的压强与温度

### 压强公式推导

考虑一个长方形容器, 长宽高分别为 $l_1$ , $l_2$ , $l_3$ , 并假设容器中有 $N$ 个同类的理想气体分子, 每个分子的质量均为 $m_0$ .

平衡状态下, 器壁各处压强一致. 考虑一个分子 $a$ 在面 $A$ (面 $A $ $\perp $ 轴 $Ox$)的撞击情况.

当分子 $a$ 即将撞击面 $A$ 时, 假设其速度为 $\vec v$ ($\vec v = \vec v_x + \vec v_y + \vec v_z$), 当分子 $a$ 撞击面 $A$ 时, 将受到来自面 $A$ , 沿 $-x$ 方向的作用力. 由于是弹性碰撞, 因此撞击后分子 $a$ 的速度变为 $\vec{v'}$ ($\vec{v'}= -\vec v_x + \vec v_y + \vec v_z$).

根据动量定理:

$$
I = (-m_0v_x)-(m_0v_x) = -2m_0v_x
$$

由于分子 $a$ 在面 $A$ 与面 $A'$ (面 $A'$也是容器壁, 且面 $A$ $\parallel$ 面 $A'$, 两面间的距离为 $l_1$)间运动速度在 $x$ 方向上分量不变, 因而分子 $a$ 与面 $A$间两次连续碰撞的间隔时间为 $\displaystyle t=\frac{2l_1}{v_x}$ . 在单位时间内, 分子 $a$ 就要与面 $A$ 有 $\displaystyle\frac{v_x}{2l_1}$ 次碰撞. 因此, 单位时间内, 分子 $a$ 受到面 $A$ 的冲量之和为: $\displaystyle-2m_0v_x\cdot\frac{v_x}{2l_1}$. 而单位时间内的冲量即为冲力, 因此分子 $a$ 对面 $A$ 的总冲力即为 $\displaystyle2m_0v_x\cdot\frac{v_x}{2l_1}$.

因此, 所有分子对面 $A$ 的碰撞以及作用在器壁上的力之和即为气体在面 $A$ 上的压力.

$$
\begin{aligned}
    F&=\sum_i \left( 2m_0v_x\cdot\frac{v_x}{2l_1} \right)\\
    &=\sum_i \frac{m_0v^2_x}{l_1}\\
    &=\frac{m_0}{l_1}\sum_i v^2_x\\
    &=\frac{Nm_0}{l_1}\cdot \operatorname{Avg}(v^2_x)
\end{aligned}
$$

按压强的定义:

$$
\begin{aligned}
    p&=\frac{F}{l_2l_3}=
    \frac{Nm_0}{l_1l_2l_3}\cdot \operatorname{Avg}(v^2_x)=
    \frac{Nm_0}{V}\cdot \operatorname{Avg}(v^2_x)\\
    \Rightarrow p&\xlongequal{n:=N/V}nm_0\cdot \operatorname{Avg}(v^2_x)
\end{aligned}
$$

### 温度的本质与统计意义