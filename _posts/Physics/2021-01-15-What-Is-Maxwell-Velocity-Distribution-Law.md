---	
layout:     post	
title:      『Physics』 What Is Maxwell Velocity Distribution Law	
subtitle:   『物理学』 何为麦克斯韦速度分布    
date:       2021-01-15	   
author:     Coekjan 
header-img: img/post-bg-PHY.jpg	
catalog:    true	
katex:    true    
tags:	
    - Physics  
---

> 本文将从理想气体物态方程与基本的力学理论导出麦克斯韦分布

## 预备结论

理想气体物态方程：

$$
pV=\frac{m}{M}RT\quad\text{or}\quad p=nkT
$$

根据理想气体的微观模型假设，气体分子的均速为零：

$$
\overline{v_x}=\overline{v_y}=\overline{v_z}=0
$$

### 压强公式

> 这部分推导在 [气体动理论](https://coekjan.github.io/2020/12/28/Kinetic-Theory-of-Gases/) 中有提及。

考虑一个长方形容器，长宽高分别为 $l_1$ , $l_2$ , $l_3$ , 并假设容器中有 $N$ 个同类的理想气体分子，每个分子的质量均为 $m_0$ .

平衡状态下，器壁各处压强一致。考虑一个分子 $a$ 在面 $A$ （面 $A $ $\perp $ 轴 $Ox$） 的撞击情况。

当分子 $a$ 即将撞击面 $A$ 时，假设其速度为 $\vec v$ ($\vec v = \vec v_x + \vec v_y + \vec v_z$), 当分子 $a$ 撞击面 $A$ 时，将受到来自面 $A$ , 沿 $-x$ 方向的作用力。由于是弹性碰撞，因此撞击后分子 $a$ 的速度变为 $\vec{v'}$ ($\vec{v'}= -\vec v_x + \vec v_y + \vec v_z$).

根据动量定理：

$$
I = (-m_0v_x)-(m_0v_x) = -2m_0v_x
$$

由于分子 $a$ 在面 $A$ 与面 $A'$ （面 $A'$也是容器壁，且面 $A$ $\parallel$ 面 $A'$, 两面间的距离为 $l_1$） 间运动速度在 $x$ 方向上分量不变，因而分子 $a$ 与面 $A$间两次连续碰撞的间隔时间为 $\displaystyle t=\frac{2l_1}{v_x}$ . 在单位时间内，分子 $a$ 就要与面 $A$ 有 $\displaystyle\frac{v_x}{2l_1}$ 次碰撞。因此，单位时间内，分子 $a$ 受到面 $A$ 的冲量之和为：$\displaystyle-2m_0v_x\cdot\frac{v_x}{2l_1}$. 而单位时间内的冲量即为冲力，因此分子 $a$ 对面 $A$ 的总冲力即为 $\displaystyle2m_0v_x\cdot\frac{v_x}{2l_1}$.

因此，所有分子对面 $A$ 的碰撞以及作用在器壁上的力之和即为气体在面 $A$ 上的压力。

$$
\begin{aligned}
    F&=\sum_i \left( 2m_0v_x\cdot\frac{v_x}{2l_1} \right)\\
    &=\sum_i \frac{m_0v^2_x}{l_1}\\
    &=\frac{m_0}{l_1}\sum_i v^2_x\\
    &=\frac{Nm_0}{l_1}\cdot \overline{v^2_x}
\end{aligned}
$$

按压强的定义：

$$
\begin{aligned}
    p&=\frac{F}{l_2l_3}=
    \frac{Nm_0}{l_1l_2l_3}\cdot \overline{v^2_x}=
    \frac{Nm_0}{V}\cdot \overline{v^2_x}\\
    \Rightarrow p&\xlongequal{n:=N/V}nm_0\cdot \overline{v^2_x}
\end{aligned}
$$

考虑到 $\overline{v^2_x}=\displaystyle\frac{1}{3}\overline{v^2}$, 压强公式即为：

$$
p=\frac{1}{3}nm_0\cdot \overline{v^2}
$$

考虑到气体分子的平均平动动能 $\overline{\varepsilon_k}=\displaystyle\frac{1}{2}m_0\cdot\overline{v^2}$, 上式还能化为：

$$
p=\frac{2}{3}n\cdot\overline{\varepsilon_k}
$$

### 温度公式

通过比对压强公式与物态方程，可以轻松得到：

$$
\overline{\varepsilon_k}=\frac{3}{2}kT
$$

这样温度 $T$ 由气体分子的平均平动动能完全确定。

### 方均根速率

考虑到 $\overline{\varepsilon_k}=\displaystyle\frac{1}{2}m\overline{v^2}$ , 可得方均根速率：

$$
\sqrt{\overline{v^2}}=\sqrt{\frac{3kT}{m}}
$$

> 注意，这里我们没有通过麦克斯韦速率分布律得到方均根速率。

## 麦克斯韦速度分布律的导出

考虑到气体分子的速度 $\vec{v}=(v_x,v_y,v_z)$ , **认为这三个分量均服从正态分布且相互独立**, 考虑到气体分子均速为零，因此正态分布的参数 $\mu=0$ . 设：

> 须指出，加粗部分的假设也可换为**气体分子在各个方向上的速度分布相同**, 可以通过微分方程解得同样结果。

$$
v_x,v_y,v_z\sim N(0,\sigma^2)
$$

可以得到这三个分量的联合密度：

$$
f_M(v_x,v_y,v_z)=\left(\frac{1}{2\pi\sigma^2}\right)^{3/2}\exp\left(-\frac{v_x^2+v_y^2+v_z^2}{2\sigma^2}\right)
$$

考虑把向量表达转化为标量形式 $v^2=v_x^2+v_y^2+v_z^2$ , 进行球坐标变换：

$$
\left\{\begin{aligned}
    v_x&=v\sin\phi\cos\theta\\
    v_y&=v\sin\phi\sin\theta\\
    v_z&=v\cos\phi
\end{aligned}\right.
$$

其 $\text{Jacobi}$ 行列式值为 $v^2\sin\phi$ .

因此：

$$
f_M(v_x,v_y,v_z)\operatorname{d}v_x\operatorname{d}v_y\operatorname{d}v_z=v^2f_M(v)\sin\phi\operatorname{d}v\operatorname{d}\phi\operatorname{d}\theta
$$

对 $\phi$ 在 $(0,\pi)$ , $\theta$ 在 $(0,2\pi)$ 下积分，得到速率密度：

$$
f(v)=4\pi\left(\frac{1}{2\pi\sigma^2}\right)^{3/2}\exp\left(-\frac{v^2}{2\sigma^2}\right)v^2
$$

接下来确定参数 $\sigma$ , 由于方均根速率 $\sqrt{\overline{v^2}}=\displaystyle\sqrt{\frac{3kT}{m}}$ , 又由于：

$$
\sqrt{\overline{v^2}}=\sqrt{\int_0^{+\infty}v^2f(v)\operatorname{d}v}
$$

积分结果与方均根速率 $\sqrt{\overline{v^2}}=\displaystyle\sqrt{\frac{3kT}{m}}$ 比对，得到：

$$
\sigma^2=\frac{kT}{m}
$$

这样，就完全给出了理想气体分子的速率分布密度：

$$
f(v)=4\pi\left(\frac{m}{2\pi kT}\right)^{3/2}\exp\left(-\frac{mv^2}{2kT}\right)v^2
$$

**这就是麦克斯韦速率分布律**.

相应地：

$$
f_M(v_x,v_y,v_z)=\left(\frac{m}{2\pi kT}\right)^{3/2}\exp\left[-\frac{m(v_x^2+v_y^2+v_z^2)}{2kT}\right]
$$

向量形式为：

$$
f_M(\vec{v})=\left(\frac{m}{2\pi kT}\right)^{3/2}\exp\left(-\frac{m\vec{v}^2}{2kT}\right)
$$

**这就是麦克斯韦速度分布律**.

## 结语

从上述推导来看，麦克斯韦速度分布律的本质就是称速度的在空间直角坐标系下的三个方向的分量均服从正态分布

$$
N\left(0,\frac{kT}{m}\right)
$$

且相互独立。