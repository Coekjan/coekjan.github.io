---	
layout:     post	
title:      『Physics』 Kinetic Theory of Gases	
subtitle:   『物理学』 气体动理论    
date:       2020-12-28	   
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

式中, $R$ 为**普适气体常量**, 在国际单位制下, $R = 8.31 \rm J/(mol\cdot K)$

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
    &\overline{\vec v_x}=
    \overline{\vec v_y}=
    \overline{\vec v_z}=
    \vec 0\\
    &\overline{v^2_x}=
    \overline{v^2_y}=
    \overline{v^2_z}=
    \frac{1}{3}\overline{v^2}
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
    &=\frac{Nm_0}{l_1}\cdot \overline{v^2_x}
\end{aligned}
$$

按压强的定义:

$$
\begin{aligned}
    p&=\frac{F}{l_2l_3}=
    \frac{Nm_0}{l_1l_2l_3}\cdot \overline{v^2_x}=
    \frac{Nm_0}{V}\cdot \overline{v^2_x}\\
    \Rightarrow p&\xlongequal{n\coloneqq N/V}nm_0\cdot \overline{v^2_x}
\end{aligned}
$$

考虑到 $\overline{v^2_x}=\displaystyle\frac{1}{3}\overline{v^2}$, 压强公式即为:

$$
p=\frac{1}{3}nm_0\cdot \overline{v^2}
$$

考虑到气体分子的平均平动动能 $\overline{\varepsilon_k}=\displaystyle\frac{1}{2}m_0\cdot\overline{v^2}$, 上式还能化为:

$$
p=\frac{2}{3}n\cdot\overline{\varepsilon_k}
$$

### 温度的本质与统计意义

考虑到 $m=Nm_0$ , $M=N_Am_0$, 理想气体的物态方程可作变换:

$$
pV=\frac{m}{M}RT\Leftrightarrow p=\frac{N}{V}\frac{R}{N_A}T\xlongequal{n\coloneqq N/V,\:k\coloneqq R/N_A}nkT
$$

其中 $n$ 是气体分子数密度, $k$ 是**玻尔兹曼常数**, 国际单位制下, $k=1.38\times 10^{-23}\rm J/K$.

将变换后的理想气体物态方程与压强公式比较, 可以得到温度公式:

$$
\begin{aligned}
    &\overline{\varepsilon_k}=\frac{3}{2}kT\\
    \Leftrightarrow&T=\frac{2}{3k}\overline{\varepsilon_k}
\end{aligned}
$$

可以看到, 该关系式把宏观量 $T$ 与微观量 $\overline{\varepsilon_k}$ 联系起来, 且说明温度 $T$ 仅与分子的平均平动动能 $\overline{\varepsilon_k}$ 成正比.

又言之, **气体温度是气体分子平均平动动能的量度**, 对于个别分子, 温度没有意义.

更有推论, **同一温度下, 各种气体分子的平均平动动能相等**.

#### 方均根速率

记气体分子的方均根速率为 $v_{rms}=\sqrt{\overline{v^2}}$ , 不难由上面推导出来的公式得到:

$$
v_{rms}=\sqrt{\frac{3kT}{m_0}}=\sqrt{\frac{3RT}{M}}
$$

注意到, 虽然同一温度下各种气体分子的平均平动动能相等, 但是**由于不同种类气体的分子质量不一致, 方均根速率会有差异**.

## 能量均分定理与理想气体的内能

### 分子的自由度

为说明分子无规则运动所遵守的统计规律, 并在这个基础上计算理想气体的内能, 须借助力学中的自由度概念.

一般分子的自由度如下表所示:

原子数 | 自由度 $i$ | 示例
:-: | :-: | :--
1 | 3 | $\rm He$
2 | 5 | $\rm H_2\quad O_2\quad N_2$
3 | 6 | $\rm H_2O$

注意, 此处我们考虑的是一般分子的自由度, 若分子中有不可旋转的化学键(如碳碳双键), 则应特殊考虑.

### 能量按自由度均分定理

根据大量分子统计平均得到, **在温度为 $T$ 的平衡态下, 气体分子的任意自由度的平均动能都等于 $\displaystyle\frac{1}{2}kT$.**

### 理想气体的内能

气体分子的能量以及分子与分子之间的势能构成气体内部的总能量, 称作**气体的内能**. 而对于理想气体, 气体分子间的使能忽略不计, 因此理想气体的内能就是所有分子的总动能.

对于理想气体, 如果一个分子具有 $i$ 个自由度, 则每个分子的平均总动能为 $\overline{\epsilon_k}=\displaystyle\frac{i}{2}kT$.

> 只考虑平动动能的话, 自由度取 $i=3$ 即得 $\overline{\varepsilon_k}=\displaystyle\frac{3}{2}kT$.

因此, 质量为 $m$ , 摩尔质量为 $M$ 的理想气体内能为:

$$
\begin{aligned}
    E&=\frac{m}{M}N_A\cdot\overline{\epsilon_k}\\
    &=\frac{m}{M}\frac{i}{2}\cdot N_A kT\xlongequal{N_Ak=R}\frac{m}{M}\frac{i}{2}RT
\end{aligned}
$$

可见, **理想气体的内能完全取决于分子运动的自由度 $i$ 和气体的温度 $T$ , 而与气体的体积, 压强无关**.

某些情况下, 也称**理想气体的内能是温度的单值函数**.

## 麦克斯韦速率分布律

### 气体分子的速率分布函数

定义分子速率分布函数:

$$
f(v)=\lim_{\Delta v\rightarrow 0} \frac{\Delta N}{N \Delta v}=\frac{\operatorname{d}N}{N\operatorname{d}v}
$$

它描述了**速率 $v$ 附近单位速率区间内分子数占总分子数的比率**. 统计学意义上, 气体分子的速率是在 $(0, +\infty)$ 上的随机分布, 分子速率分布函数 $f(v)$ 本质上是一个**概率密度函数**.

若已知分子速率分布函数 $f(v)$ , 要计算速率处于 $(v_1, v_2)$ 间的分子数目占总数的比率, 只需进行积分:

$$
\eta=\frac{\Delta N}{N}=\int_{v_1}^{v_2}f(v)\operatorname{d}v
$$

若速率区间极小, 可以用近似方法计算分子数:

$$
\Delta N = Nf(v)\Delta v
$$

显然, 该函数满足**归一化条件**:

$$
\int_0^{+\infty}f(v)\operatorname{d}v=1
$$

### 麦克斯韦速率分布律

麦克斯韦从理论上导出了理想气体在平衡态时气体分子的速率分布函数:

$$
f(v)=4\pi\left(\frac{m_0}{2\pi k T}\right)^{3/2}\exp\left(-\frac{m_0v^2}{2kT}\right)v^2
$$

对麦克斯韦分布律进行一些变换, 得到**动能分布概率密度**.

$$
\begin{aligned}
    \epsilon_k&=\frac{1}{2}m_0v^2
    &&&
    \operatorname{d}\epsilon_k&=m_0v\operatorname{d}v\\
    \Rightarrow
    v&=\sqrt{\frac{2\epsilon_k}{m_0}}
    &&&
    \operatorname{d}v&=\frac{\operatorname{d}\epsilon_k}{\sqrt{2m_0\epsilon_k}}
\end{aligned}
$$

代入麦克斯韦速率分布律, 即得:

$$
f(\epsilon_k)=\frac{2}{\sqrt{\pi}}\left(\frac{1}{kT}\right)^{3/2}\exp\left(-\frac{\epsilon_k}{kT}\right)\epsilon_k^{1/2}
$$

#### 麦克斯韦速度分布律

麦克斯韦一开始提出的是麦克斯韦**速度**分布律为:

$$
f_M(\vec v)=\left(\frac{m_0}{2\pi k T}\right)^{3/2}\exp\left(-\frac{m_0{\vec v}^2}{2kT}\right)
$$

可通过速度分量写作标量形式:

$$
f_M(v_x, v_y, v_z)=\left(\frac{m_0}{2\pi k T}\right)^{3/2}\exp\left[-\frac{m_0(v_x^2+v_y^2+v_z^2)}{2kT}\right]
$$

> 本质上, 是称 $v_x, v_y, v_z \thicksim N(0, \displaystyle\frac{kT}{m_0})$ , 且三者相互独立. 则有:
> 
> $$
> \left\{\begin{aligned}
>   f_M^{(x)}(v_x)&=\left(\frac{m_0}{2\pi k T}\right)^{1/2}\exp\left(-\frac{m_0v_x^2}{2kT}\right)\\
    f_M^{(y)}(v_y)&=\left(\frac{m_0}{2\pi k T}\right)^{1/2}\exp\left(-\frac{m_0v_y^2}{2kT}\right)\\
    f_M^{(z)}(v_z)&=\left(\frac{m_0}{2\pi k T}\right)^{1/2}\exp\left(-\frac{m_0v_z^2}{2kT}\right)
> \end{aligned}\right.\Rightarrow f_M(v_x, v_y, v_z) = f_M^{(x)}(v_x)f_M^{(y)}(v_y)f_M^{(z)}(v_z)
> $$
> 

那么可以给出任意速度附近的分布概率密度:

$$
\operatorname{d}w=f_M(v_x, v_y, v_z)\operatorname{d}v_x\operatorname{d}v_y\operatorname{d}v_z
$$

考虑球坐标变换: $\operatorname{d}v_x\operatorname{d}v_y\operatorname{d}v_z\rightarrow\operatorname{d}v\operatorname{d}\phi\operatorname{d}\theta$ , 变换关系为:

$$
\left\{\begin{aligned}
    v_x&=v\sin \phi \cos \theta\\
    v_y&=v\sin \phi \sin \theta\\
    v_z&=v\cos \phi
\end{aligned}\right.
$$

其Jacobi行列式为:

$$
J=\begin{vmatrix}
    \displaystyle\frac{\partial v_x}{\partial v} & \displaystyle\frac{\partial v_x}{\partial \phi} & 
    \displaystyle\frac{\partial v_x}{\partial \theta}\\\\
    \displaystyle\frac{\partial v_y}{\partial v} & \displaystyle\frac{\partial v_y}{\partial \phi} & 
    \displaystyle\frac{\partial v_y}{\partial \theta}\\\\
    \displaystyle\frac{\partial v_z}{\partial v} & \displaystyle\frac{\partial v_z}{\partial \phi} & 
    \displaystyle\frac{\partial v_z}{\partial \theta}
\end{vmatrix}=v^2\sin \phi
$$

变换后对 $\phi$ 和 $\theta$ 积分, 即可得到任意速率附近的分布概率密度:

$$
\operatorname{d}w=4\pi f_M(v)v^2\operatorname{d}v
$$

其中即可得到麦克斯韦速率分布律:

$$
f(v)=4\pi f_M(v) v^2=4\pi\left(\frac{m_0}{2\pi k T}\right)^{3/2}\exp\left(-\frac{m_0v^2}{2kT}\right)v^2
$$

#### 分子速率的统计平均值

任意有关速率的统计量 $g(v)$ 的平均值, 均可由积分:

$$
\overline{g(v)}=\int_0^{+\infty}g(v)f(v)\operatorname{d}v
$$

给出.

对于麦克斯韦速率分布律, 列出以下几个简单的统计量.

方均根速率:

$$
v_{rms}
=\sqrt{\int_0^{+\infty}v^2f(v)\operatorname{d}v}
=\sqrt{\frac{3kT}{m_0}}
=\sqrt{\frac{3RT}{M}}
=\sqrt{3}\sqrt{\frac{RT}{M}}
\approx1.73\sqrt{\frac{RT}{M}}
$$

平均速率:

$$
\overline{v}
=\int_0^{+\infty}vf(v)\operatorname{d}v
=\sqrt{\frac{8kT}{\pi m_0}}
=\sqrt{\frac{8RT}{\pi M}}
=\sqrt{\frac{8}{\pi}}\sqrt{\frac{RT}{M}}
\approx1.60\sqrt{\frac{RT}{M}}
$$

最概然速率(概率密度最大处对应的速率):

$$
v_p
=\sqrt{\frac{2kT}{m_0}}
=\sqrt{\frac{2RT}{M}}
=\sqrt{2}\sqrt{\frac{RT}{M}}
\approx 1.44\sqrt{\frac{RT}{M}}
$$

可以看到, 这三个统计量有大小关系:

$$
v_{rms}>\overline{v}>v_p
$$

### 玻尔兹曼分布律与MB分布

在保守场中, 粒子密度将按位置分布:

$$
n=n_0\exp\left(-\frac{\epsilon_p}{kT}\right)
$$

其中, $n_0$ 是 $\epsilon_p=0$ 时的粒子数密度.

综合麦克斯韦与玻尔兹曼的成果, 即得**麦克斯韦-玻尔兹曼分布(MB分布)**. 经典力学中, 微粒的状态由位置与动量确定, 因此粒子按速度分布与按位置分布是相互独立的, 可以得到MB分布的概率密度函数:

$$
\begin{aligned}
    \left\{\begin{aligned}
        f_M
    &=\left(\frac{m_0}{2\pi k T}\right)^{3/2}\exp\left(-\frac{\epsilon_k}{kT}\right)\\
    f_B
    &=n_0\exp\left(-\frac{\epsilon_p}{kT}\right)
    \end{aligned}\right.
\end{aligned}
$$

$$
    \Rightarrow f = f_M\cdot f_B=n_0\left(\frac{m_0}{2\pi k T}\right)^{3/2}\exp\left(-\frac{\epsilon_k+\epsilon_p}{kT}\right)
$$

显然, 上式若对位置积分就是麦克斯韦分布, 若对速度积分就是玻尔兹曼分布.

#### 玻尔兹曼分布的应用

应用玻尔兹曼分布, 可以得到重力场中微粒密度随高度的分布:

$$
n=n_0\exp\left(-\frac{mgz}{kT}\right)
$$

若使用 $p=nkT$ , 则可以得到等温气压公式:

$$
p=p_0\exp\left(-\frac{mgz}{kT}\right)
$$

## 分子平均自由程

我们把分子 $1s$ 内和其他分子碰撞的次数称为**平均碰撞频率**(简称**碰撞频率**), 用 $\overline Z$ 表示; 把分子两侧连续碰撞间一个分子自由运动的平均路程称为**平均自由程**, 用 $\overline \lambda$ 表示.

### 平均自由程公式

#### 碰撞频率公式的导出

为导出平均自由程公式, 下面先导出碰撞频率公式.

假设每个分子都是直径为 $d$ 的小球. 由于碰撞考虑的是相对运动, 假设除了研究对象外的分子均静止, 只有研究对象以平均相对速率 $\overline{v_r}$ 运动.

由于运动分子与其他分子之间的距离小于 $d$ 时才碰撞, 因此运动分子在单位时间内扫过一个长度为 $\overline{v_r}$ , 横截面为 $\pi d^2$ 的**碰撞作用圆柱体**. 圆柱体因碰撞曲折而发生的体积变化在平均自由程很大时可以忽视.

假设单位体积内分子数目为 $n$ , 则静止分子中心在圆柱体内的数目为 $n\pi d^2\overline{v_r}$ . 因中心在圆柱体内的所有分子都将与运动分子碰撞, 因此分子的平均碰撞频率为 $\overline Z = n\pi d^2\overline{v_r}$ .

为进一步导出碰撞频率与分子平均速率间的关系, 下面推导分子平均相对速率与平均速率间的关系.

##### 分子平均相对速率与平均速率的关系

处于平衡态的理想气体, 粒子按速度分布的规律为麦克斯韦分布律, 

$$
\operatorname{d}w=\left(\frac{m_0}{2\pi k T}\right)^{3/2}\exp\left(-\frac{m_0{\vec v}^2}{2kT}\right)\operatorname{d}\vec v
$$

考虑两个分子 $a_1$ 与 $a_2$ . $a_1$ 速度为 $\vec {v_1}$ , $a_2$ 速度为 $\vec {v_2}$ 的概率分别为 $\operatorname{d}w_1$ , $\operatorname{d}w_2$ , 且两者相互独立, 可以得到两者同时发生的概率为:

$$
\operatorname{d}w
=\operatorname{d}w_1\operatorname{d}w_2
=\left(\frac{m_0}{2\pi k T}\right)^3\exp\left[-\frac{m_0({\vec {v_1}}^2+{\vec {v_2}}^2)}{2kT}\right]\operatorname{d}\vec {v_1}\operatorname{d}\vec {v_2}
$$

两分子的相对速度为 $\vec {v_r}=\vec {v_1} - \vec {v_2}$ . 以这两个分子为研究对象, 其质心速度 $\vec{u_c}=\displaystyle\frac{\vec{v_1}+\vec{v_2}}{2}$ .

因此:

$$
\begin{aligned}
    \vec {v_1}=\vec {u_c}+\frac{\vec {v_r}}{2}\\
    \vec {v_2}=\vec {u_c}-\frac{\vec {v_r}}{2}
\end{aligned}
$$

因此:

$$
\vec {v_1}^2+\vec {v_2}^2=2\vec {u_c}^2+\frac{\vec {v_r}^2}{2}
$$

欲作变换 $\operatorname{d}\vec {v_1}\operatorname{d}\vec {v_2}\rightarrow\operatorname{d}\vec {u_c}\operatorname{d}\vec {v_r}$ , 须求Jacobi行列式:

$$
J=\begin{vmatrix}
    \displaystyle\frac{\partial\vec{v_1}}{\partial\vec{v_r}} & \displaystyle\frac{\partial\vec{v_1}}{\partial\vec{u_c}} \\\\
    \displaystyle\frac{\partial\vec{v_2}}{\partial\vec{v_r}} & \displaystyle\frac{\partial\vec{v_2}}{\partial\vec{u_c}} \\
\end{vmatrix}=\begin{vmatrix}
    \displaystyle\frac{1}{2} & 1 \\\\
    \displaystyle-\frac{1}{2} & 1
\end{vmatrix}=1
$$

因此 $\operatorname{d}\vec {v_1}\operatorname{d}\vec {v_2}=\operatorname{d}\vec {u_c}\operatorname{d}\vec {v_r}$ .

因此:

$$
\begin{aligned}
    \operatorname{d}w
    &=\operatorname{d}w_1\operatorname{d}w_2
    =\left(\frac{m_0}{2\pi k T}\right)^3\exp\left[-\frac{m_0({\vec {v_1}}^2+{\vec {v_2}}^2)}{2kT}\right]\operatorname{d}\vec {v_1}\operatorname{d}\vec {v_2}\\
    &=
    \left(\frac{2m_0}{2\pi k T}\right)^{3/2}\exp\left(-\frac{2m_0{\vec {u_c}}^2}{2kT}\right)\operatorname{d}\vec {u_c}
    \cdot
    \left(\frac{m_0/2}{2\pi k T}\right)^{3/2}\exp\left(-\frac{m_0{\vec {v_r}}^2/2}{2kT}\right)\operatorname{d}\vec {v_r}\\
    &=
    f(\vec {u_c})\operatorname{d}\vec {u_c}
    \cdot
    f(\vec {v_r})\operatorname{d}\vec {v_r}
\end{aligned}
$$

即得:

$$
f(\vec {u_c})=\left(\frac{2m_0}{2\pi k T}\right)^{3/2}\exp\left(-\frac{2m_0{\vec {u_c}}^2}{2kT}\right)
$$

$$
f(\vec {v_r})=\left(\frac{m_0/2}{2\pi k T}\right)^{3/2}\exp\left(-\frac{m_0{\vec {v_r}}^2/2}{2kT}\right)
$$

由于:

$$
\begin{aligned}
    \int_0^{+\infty}f(\vec {u_c})\operatorname{d}\vec {u_c}
    &=\int_0^{+\infty}4\pi \left(\frac{2m_0}{2\pi k T}\right)^{3/2}\exp\left(-\frac{2m_0u_c^2}{2kT}\right)u_c^2\operatorname{d}u_c\\
    &=\int_0^{+\infty}4\pi \left(\frac{m_0}{2\pi k T}\right)^{3/2}\exp\left[-\frac{m_0(\sqrt2u_c)^2}{2kT}\right](\sqrt2u_c)^2\operatorname{d}(\sqrt2u_c)=1\\
    \int_0^{+\infty}f(\vec {v_r})\operatorname{d}\vec {v_r}
    &=\int_0^{+\infty}4\pi \left(\frac{m_0/2}{2\pi k T}\right)^{3/2}\exp\left(-\frac{m_0v_r^2/2}{2kT}\right)v_r^2\operatorname{d}v_r\\
    &=\int_0^{+\infty}4\pi \left(\frac{m_0}{2\pi k T}\right)^{3/2}\exp\left[-\frac{m_0(v_r/\sqrt2)^2}{2kT}\right] \left(\frac{v_r}{\sqrt{2}}\right)^2\operatorname{d}\left(\frac{v_r}{\sqrt{2}}\right)=1
\end{aligned}
$$

因此验证, 函数 $f(v_r)=\displaystyle4\pi \left(\frac{m_0/2}{2\pi k T}\right)^{3/2}\exp\left(-\frac{m_0v_r^2/2}{2kT}\right)v_r^2$ 即为分子间的**相对速率分布概率密度**.

据此, 可以计算平均相对速率:

$$
\begin{aligned}
    \overline{v_r}
    &=\int_0^{+\infty}v_rf(v_r)\operatorname{d}v_r\\
    &=\int_0^{+\infty}4\pi \left(\frac{m_0/2}{2\pi k T}\right)^{3/2}\exp\left(-\frac{m_0v_r^2/2}{2kT}\right)v_r^3\operatorname{d}v_r\\
    &=\sqrt{2}\int_0^{+\infty}4\pi \left(\frac{m_0}{2\pi k T}\right)^{3/2}\exp\left[-\frac{m_0(v_r/\sqrt{2})^2}{2kT}\right]\left(\frac{v_r}{\sqrt{2}}\right)^3\operatorname{d}\left(\frac{v_r}{\sqrt{2}}\right)\\
    &\xlongequal{v\coloneqq v_r/\sqrt{2}}\sqrt{2}\int_0^{+\infty}4\pi \left(\frac{m_0}{2\pi k T}\right)^{3/2}\exp\left[-\frac{m_0v^2}{2kT}\right]v^3\operatorname{d}v\\
    &=\sqrt{2}\overline{v}
\end{aligned}
$$

推得: $\overline{v_r}=\sqrt{2}\overline{v}$

##### 平均碰撞频率公式

利用以上关系, 可以最终算得:

$$
\overline Z = \sqrt{2}n\pi d^2\overline{v}
$$

#### 平均自由程公式的导出

单位时间内, 分子运动平均经过的路程为 $\overline{v}$ . 因此平均自由程为:

$$
\overline \lambda = \frac{\overline{v}}{\overline Z} = \frac{1}{\sqrt{2}n\pi d^2}
$$

利用 $p=nkT$ 即得:

$$
\overline \lambda = \frac{kT}{\sqrt{2}\pi d^2p}
$$

可见, 温度一定时, $\overline\lambda$ 与 $p$ 成反比.

另外, 实验表明, 气体密度一定时, $d$ 将随速度增加而减小, 因此当 $T$ 与 $p$ 比值一定时, $\overline\lambda$ 将随温度 $T$ 升高而略有增大.