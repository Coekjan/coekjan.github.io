---	
layout:     post	
title:      『Physics』 Mechanical Wave	
subtitle:   『物理学』 机械波    
date:       2020-12-31	   
author:     Coekjan 
header-img: ""	
catalog:    true	
katex:    true    
tags:	
    - Physics  
---

## 机械波简介

### 横波与纵波

**机械振动**以一定速度在弹性介质中由近及远地传播, 形成**机械波**. 产生机械波需要有两个条件: **波源**, **弹性介质**.

按介质中质元的振动方向和波在介质中传播的方向之间的关系, 可以把波分成横波与纵波.

若质元的振动方向和波的传播方向相互垂直, 这种波称为**横波**; 若质元的振动方向和波的传播方向相互平行, 这种波称为**纵波**.

### 产生机械横波与机械纵波的介质条件

**只有固体**才能传播**机械横波**: 固体能够发生切变, 液体和气体不能.

**固体, 液体和气体**都能传播**机械纵波**: 固体, 液体和气体都能产生压缩和膨胀形变.

### 波的几何描述

**波面**: 在波的传播过程中, 任一时刻介质中振动相位相同的点连结形成的面. 也称之为**波阵面**.

**波线**: 波的传播方向. 也称之为**波阵面**.

**波前**: 某一时刻, 波传播到的最前面的波面.

> 在各向同性的均匀介质中, 波线与波面垂直.

### 波的特征量

**波长**: 同一波线上两个相邻的, 相位差为 $2\pi$ 的质元之间的距离, 用 $\lambda$ 表示.

**周期**, **频率**: 波前进一个波长所需的时间称为波的周期, 用 $T$ 表示; 周期的倒数称为波的频率, 用 $\nu$ 表示.

**波速**: 单位时间内振动状态传播的距离, 用 $u$ 表示. 又称**相速**.

这四个特征量有以下关系:

$$
u=\frac{\lambda}{T}=\nu\lambda
$$

**角波数**: $2\pi$ 距离内波的数目, 用 $k$ 表示. $\displaystyle k=\frac{2\pi}{\lambda}$ .

## 简谐波

若介质传播的是谐振动, 且波的所到之处, 介质中各个质点作同频率的谐振动, 则称这样的波为**简谐波**. 若简谐波的波面为平面, 则称之为**平面简谐波**.

### 平面简谐波的波函数

一般的波, 可以用**波函数** $y=f(x,t)$ 描述. 对于向 $+x$ 方向传播的平面简谐波, 我们有:

$$
y(x,t)=A\cos(\omega t - kx+\phi_0)
$$

通过特征量之间的关系, 可以得到平面简谐波波函数的其他表达形式:

$$
\left.\begin{aligned}
    y&=A\cos\left[\omega\left(t\mp\frac{x}{u}\right)+\phi_0\right]\\
    y&=A\cos\left[2\pi\left(\frac{t}{T}\mp\frac{x}{\lambda}\right)+\phi_0\right]\\
    y&=A\cos\left[2\pi\left(\nu t\mp\frac{x}{\lambda}\right)+\phi_0\right]\\
    y&=A\cos(\omega t\mp kx+\phi_0)\\
    y&=A\cos\left(\omega t\mp\frac{2\pi x}{\lambda}+\phi_0\right)
\end{aligned}\right\}
$$

### 波动方程

下式反映了一切波 $\xi=\xi(x,y,z,t)$ 的传播特征:

$$
\frac{\partial^2\xi}{\partial x^2}+\frac{\partial^2\xi}{\partial y^2}+\frac{\partial^2\xi}{\partial z^2}=\frac{1}{u^2}\frac{\partial^2\xi}{\partial t^2}
$$

这是以 $u$ 为传播速度的波动过程.

## 波的能量与波的强度

### 波的能量

以线密度(单位长度的质量)为 $\rho_l$ 的弦线上传播平面简谐波为例, 设波函数:

$$
y=A\cos\left[\omega\left(t-\frac{x}{u}\right)+\phi_0\right]
$$

取 $x$ 处长为 $\Delta x$ 的线元, 其动能为:

$$
\Delta E_k=\frac{1}{2}\rho_l\Delta x\left(\frac{\partial y}{\partial t}\right)^2
$$

弦线上的张力, 使得线元由原长 $\Delta x$ 变为 $\Delta l$ , 即伸长了 $\Delta l - \Delta x$ , 其弹性势能应等于 $F$ (近似以静止弦张力计算) 在线元伸长过程中做的功:

$$
\Delta E_p=F(\Delta l - \Delta x)
$$

在 $\Delta x$ 很小时:

$$
\begin{aligned}
    \Delta l&=\sqrt{(\Delta x)^2+(\Delta y)^2}=\Delta x\sqrt{1+\left(\frac{\Delta y}{\Delta x}\right)^2}\\
    &\approx\Delta x\sqrt{1+\left(\frac{\partial y}{\partial x}\right)^2}\approx\Delta x\left[1+\frac{1}{2}\left(\frac{\partial y}{\partial x}\right)^2\right]
\end{aligned}
$$

得到:

$$
\Delta E_p\approx\frac{1}{2}F\Delta x\left(\frac{\partial y}{\partial x}\right)^2
$$

由于是平面简谐波, 因此:

$$
\left\{\begin{aligned}
    \frac{\partial y}{\partial t}&=-\omega A\sin\left[\omega\left(t-\frac{x}{u}\right)+\phi_0\right]\\
    \frac{\partial y}{\partial x}&=\frac{\omega}{u}A\sin\left[\omega\left(t-\frac{x}{u}\right)+\phi_0\right]
\end{aligned}\right.
$$

可以得到:

$$
\begin{aligned}
    \Delta E_k&=\frac{1}{2}\rho_l\Delta x\omega^2A^2\sin^2\left[\omega\left(t-\frac{x}{u}\right)+\phi_0\right]\\
    \Delta E_p&=\frac{1}{2}F\Delta x\frac{\omega^2}{u^2}A^2\sin^2\left[\omega\left(t-\frac{x}{u}\right)+\phi_0\right]
\end{aligned}
$$

利用弦上横波的波速公式: $u=\displaystyle\sqrt{\frac{F}{\rho_l}}$ , 可以对 $\Delta E_p$ 进行化简:

$$
\Delta E_p=\frac{1}{2}\rho_l\Delta x\omega^2A^2\sin^2\left[\omega\left(t-\frac{x}{u}\right)+\phi_0\right]\Rightarrow\Delta E_k=\Delta E_p
$$

总机械能:

$$
\Delta E=\Delta x\rho_l\omega^2A^2\sin^2\left[\omega\left(t-\frac{x}{u}\right)+\phi_0\right]
$$

可以得到结论:
1. 介质中任一质元的动能和势能是同步变化的, 即 $\Delta E_k=\Delta E_p$ ;
2. 质元机械能 $\Delta E$ 随时间变化, 波动过程是能量的传播过程;
3. 质元的机械能为 $\displaystyle\Delta E=\Delta x\rho_l\omega^2A^2\sin^2\left[\omega\left(t-\frac{x}{u}\right)+\phi_0\right]$ .

引入**能量密度**: 介质中单位体积的波动能量, 用 $w$ 表示. 若质元的横截面为 $S$ , 体密度为 $\rho=\displaystyle\frac{\rho_l}{S}$ , 则能量密度:

$$
w=\frac{\Delta E}{S\Delta x}=\rho\omega^2A^2\sin^2\left[\omega\left(t-\frac{x}{u}\right)+\phi_0\right]
$$

能量密度随时间周期性变化, 取其一周期内平均值, 得到**平均能量密度**:

$$
\overline{w}=\frac{1}{T}\int_0^Tw\operatorname{d}t=\frac{1}{2}\rho A^2\omega^2
$$

### 波的强度

引入**能流**: 单位时间内通过介质中某截面的能量, 用 $P$ 表示. 若介质中某截面的面积为 $S$ , 波速为 $u$, 能量密度为 $w$ , 则:

$$
P=wuS
$$

同样, 能流也随时间周期性变化, 取其一周期内的平均值, 得到平均能量密度:

$$
\overline{P}=\frac{1}{T}\int_0^TP\operatorname{d}t=\overline{w}uS
$$

引入**平均能流密度**: 通过与波动传播方向垂直的单位面积上的平均能流, 又称**波的强度**, 用 $I$ 表示:

$$
I=\overline{w}u=\frac{1}{2}\rho u\omega^2 A^2\xlongequal{Z:=\rho u}\frac{1}{2}Z\omega^2A^2
$$

式中 $Z$ 称为**特性阻抗**. 该式表明:
1. $I\propto A^2$ ;
2. $I\propto \omega^2$ ;
3. $I\propto Z$ .

### 球面波

**波在传播过程中, 若介质不吸收能量, 则波通过两个不同的面时的总能流应相等**. 球面波中, 即为:

$$
\frac{1}{2}\rho u\omega^2A_1^2\cdot4\pi r_1^2=\frac{1}{2}\rho u\omega^2A_2^2\cdot4\pi r_2^2\Rightarrow\frac{A_1}{A_2}=\frac{r_2}{r_1}
$$

即振幅与离开波源的距离成反比, 因此球面简谐波的表达式为:

$$
\xi=\frac{A_0r_0}{r}\cos\left[\omega\left(t-\frac{r}{u}\right)+\phi_0\right]
$$

其中, $r:=\sqrt{x^2+y^2+z^2}$ , $A_0$ 为 $r_0$ 处的振幅.

## 惠更斯原理

惠更斯于1678年提出了关于波传播的几何法则: **在波的传播过程中, 波阵面(波前)上的每一点都可以看做是发射子波的波源, 在其后的任一时刻, 这些子波的包迹就成为新的波阵面**. 这就是**惠更斯原理**.

波在传播过程中, 若遇到障碍物, 其传播方向绕过障碍物发生偏折的现象, 称为**波的衍射**.

通过惠更斯原理, 可以导出折射定律:

$$
\frac{\sin i}{\sin\gamma}=\frac{u_1\Delta t}{u_2\Delta t}=n_{21}
$$

## 波的叠加与波的干涉

### 波的叠加原理

若有几列波同时在一介质中传播, 如果这几列波在空间中相遇, 那么它们将维持自身原有的特性(频率, 波长, 振动方向等)**独立传播**.

在相遇的区域内, 任一质元的振动为各列波单独在该点引起的振动的合振动, 这就是**波的叠加原理**.

### 波的干涉

#### 波的干涉现象

当两列或多列波叠加时, 其合振动的振幅 $A$ 和合强度 $I$ 将在空间形成一种稳定的分布, 即某些点上的振动始终加强, 某些点上的振动始终减弱的现象.

**相干条件**: 频率相同, 振动方向相同, 相位差恒定.

#### 干涉规律

考虑由波源 $S_1$ 和 $S_2$ 产生的波在空间点 $P$ 处相遇, 两列波在该点处引起的振动表达式为:

$$
\left\{\begin{aligned}
    y_1&=A_1\cos\left(\omega t+\phi_{01}-\frac{2\pi r_1}{\lambda}\right)\\
    y_2&=A_2\cos\left(\omega t+\phi_{02}-\frac{2\pi r_2}{\lambda}\right)
\end{aligned}\right.
$$

根据叠加原理, $P$ 处的合振动为:

$$
y=y_1+y_2=A\cos(\omega t+\phi_0)
$$

其中:

$$
\begin{aligned}
    A&=\sqrt{A_1^2+A_2^2+2A_1A_2\cos\left(\phi_{02}-\phi_{01}-2\pi\frac{r_2-r_1}{\lambda}\right)}\\
    \phi_0&=\arctan\frac{
        A_1\sin\left(\phi_{01}-\displaystyle\frac{2\pi r_1}{\lambda}\right)+
        A_2\sin\left(\phi_{02}-\displaystyle\frac{2\pi r_2}{\lambda}\right)}{
        A_1\cos\left(\phi_{01}-\displaystyle\frac{2\pi r_1}{\lambda}\right)+
        A_2\cos\left(\phi_{02}-\displaystyle\frac{2\pi r_2}{\lambda}\right)}
\end{aligned}
$$

这两列波在空间任意一点的振动相位差: $\Delta\phi=\phi_{02}-\phi_{01}-2\pi\displaystyle\frac{r_2-r_1}{\lambda}$ 是一个定值.

根据 $A$ 的表达式, 得到相干相涨条件:

$$
\Delta \phi=2k\pi\quad (k=0,\pm1,\pm2,\dotsm)
$$

相干相消条件:

$$
\Delta \phi=(2k+1)\pi\quad (k=0,\pm1,\pm2,\dotsm)
$$

若 $\phi_{02}=\phi_{01}$ , 则上述条件得到简化.

相干相涨条件:

$$
\delta=r_1-r_2=k\lambda\quad (k=0,\pm1,\pm2,\dotsm)
$$

相干相消条件:

$$
\delta=r_1-r_2=\left(k+\frac{1}{2}\right)\lambda\quad (k=0,\pm1,\pm2,\dotsm)
$$

关于叠加后波的强度:

$$
I=I_1+I_2+2\sqrt{I_1I_2}\cos\Delta\phi
$$

若两列波强度相等 $I_1=I_2=I_0$ , 那么有:

$$
I=4I_0\cos^2\frac{\Delta\phi}{2}
$$

#### 驻波

若有两列振幅相同, 相向传播的相干波, 它们叠加的结果即为**驻波**.

考虑如下两列波:

$$
\left\{\begin{aligned}
    y_1&=A\cos2\pi\left(\nu t-\frac{x}{\lambda}\right)\\
    y_2&=A\cos2\pi\left(\nu t+\frac{x}{\lambda}\right)
\end{aligned}\right.
$$

叠加结果:

$$
\begin{aligned}
    y&=y_1+y_2=A\left[\cos2\pi\left(\nu t-\frac{x}{\lambda}\right)+\cos2\pi\left(\nu t+\frac{x}{\lambda}\right)\right]\\
    &=2A\cos2\pi\frac{x}{\lambda}\cos2\pi\nu t
\end{aligned}
$$

波腹: $\displaystyle\left\vert\cos\frac{2\pi x}{\lambda}\right\vert=1\Leftrightarrow x=k\frac{\lambda}{2}\quad (k=0,\pm1,\pm2,\dotsm)$

波节: $\displaystyle\left\vert\cos\frac{2\pi x}{\lambda}\right\vert=0\Leftrightarrow x=(2k+1)\frac{\lambda}{4}\quad (k=0,\pm1,\pm2,\dotsm)$

> 波节点将介质划分为长为 $\displaystyle\frac{\lambda}{2}$ 的许多段, 每段中振幅不同但相位相同; 而相邻段间各质点的振动相位相反. 即驻波中不存在相位传播, 也就没有能量的传播.

##### 半波损失

在波垂直入射的情况下, 把密度 $\rho$ 与波速 $u$ 的乘积 $\rho u$ 较大的称为**波密介质**, 较小的称为 **波疏介质** .

当波从波疏介质传播到波密介质, 在分界处反射时, 反射点是波节, 有相位 $\pi$ 的突变, 称作**半波损失**;

当波从波密介质传播到波疏介质, 在分界处反射时, 相位没有突变, 没有半波损失.

## 多普勒效应

由于观察者(接收器)或波源, 或两个同时相对介质运动, 而使得观察者接收到的频率与波源发出的频率不同的现象, 称为**多普勒效应**.

若波源波速为 $u$ , 频率为 $\nu_s$ . 作以下三种情况讨论.

### 仅观察者相对于介质运动

设观察者相对于介质的速度为 $v_o$ . 规定观察者接近波源时取正, 远离波源时取负.

不难有结论:

$$
\nu=\frac{u+v_o}{u}\nu_0
$$

### 仅波源相对于介质运动

设波源相对于介质的速度为 $v_s$ . 规定波源接近观察者时取正, 远离观察者时取负.

不难有结论

$$
\nu=\frac{u}{u-v_s}\nu_0
$$

### 观察者与波源同时相对于介质运动

直接综合上述两种情况:

$$
\nu=\frac{u+v_o}{u-v_s}\nu_0
$$

分析 $v_o$ 和 $v_s$ 的符号时:
1. 若波源和观察者相向运动, 应均为正;
2. 若波源和观察者相背运动, 应均取负;
3. 若波源和观察者同向运动:
   * 若观察者速度方向指向波源, 则 $v_o$ 取正, $v_s$ 取负;
   * 若观察者速度方向背离波源, 则 $v_o$ 取负, $v_s$ 取正.

### 电磁波的多普勒效应

考虑到相对论效应, 可以得到:

$$
\nu=\sqrt{\frac{c+v}{c-v}}\nu_0
$$

式中 $v$ 为波源和接受器间的相对运动速度, 两者相互靠近时取正, 相互远离时取负.

相互靠近时, 观察者接收到的电磁波频率比波源频率高, 称为**紫移**;

相互远离时, 观察者接受到的电磁波频率比波源频率低, 称为**红移**.