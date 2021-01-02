---	
layout:     post	
title:      『Physics』 Basis of Quantum Physics	
subtitle:   『物理学』 量子物理基础    
date:       2021-01-02	   
author:     Coekjan 
header-img: img/post-bg-PHY.jpg	
catalog:    true	
katex:    true    
tags:	
    - Physics  
---

## 热辐射

由于物体中的分子, 原子受到热激发而发射电磁波的现象为**热辐射**. 物体向四周发射的能量称为**辐射能**.

物体不仅能辐射电磁波, 而且还能吸收和反射电磁波. 如果辐射的能量恰好等于同一时间内吸收的能量, 此时物体温度保持不变, 称这种辐射为**平衡热辐射**.

引入**辐射出射度**: 在温度 $T$ 时, 单位时间内从物体单位表面积上所发射的各种波长的总辐射能, 简称**辐出度**. 常用 $M(T)$ 表示, 单位为 $\rm W/m^2$ .

引入**单色辐出度**: 在温度 $T$ 时, 单位时间内从物体单位表面积上所发射的, 波长处于 $\lambda\sim\lambda+\operatorname{d}\lambda$ 范围内的单位波长间隔中的辐射能. 用 $M(\lambda, T)$ 或 $M_\lambda(T)$ 表示, 单位为 $\rm W/m^3$ .

不难发现:

$$
M(\lambda, T)=\frac{\operatorname{d}M(T)}{\operatorname{d}\lambda}
$$

引入**单色吸收比**: 在温度 $T$ 时, 物体吸收波长在 $\lambda\sim\lambda+\operatorname{d}\lambda$ 范围内辐射能与相应波长范围内入射的电磁能量之比. 用 $a(\lambda, T)$ 表示, 其值在 $(0,1)$ 内. 若物体在任何温度下, 对任何波长的吸收比都是 $1$ , 则称这种物体为**黑体**.

现实中, 黑体不存在. 而实验中, 使用带小孔的空腔模拟黑体.

理论和实验表明: 物体的辐射本领越大, 其吸收本领也越大.

基尔霍夫提出: **在同样温度的热平衡条件下, 各种物体对相同波长的单色辐出度与单色吸收比的比值 $\displaystyle\frac{M(\lambda, T)}{a(\lambda, T)}$ 都相等, 等于同温度下黑体对同意波长的单色辐出度.** 黑体的单色辐出度用 $M_0(\lambda, T)$ 表示.

### 黑体辐射实验定律

利用黑体模型, 可以用实验方法测出黑体的单色辐出度 $M_0(\lambda, T)$ 随 $\lambda$ 和 $T$ 变化的实验曲线.

#### 斯特藩-玻尔兹曼定律

曲线下面积等于黑体在该温度下的总辐出度:

$$
M_0(T)=\int_0^{+\infty}M_0(\lambda, T)\operatorname{d}\lambda
$$

实验表明: $M_0(T)$ 随温度增高而迅速增加, 黑体的辐出度与其温度 $T$ 的四次方成正比:

$$
M_0(T)=\sigma T^4
$$

这就是**斯特藩-玻尔兹曼定律**, 其中比例系数 $\sigma$ 为**斯特藩常量**:

$$
\sigma=5.67\times10^{-8}\rm W/(m^2\cdot K^4)
$$

#### 维恩位移定律

曲线的极大值对应的波长 $\lambda_\text{m}$ 随着温度 $T$ 的增高而向短波方向移动:

$$
T\lambda_\text{m}=b
$$

这就是**维恩位移定律**, 其中 $b$ 为**维恩常量**:

$$
b=2.90\times10^{-3}\rm m\cdot K
$$

#### 普朗克公式

为得到黑体辐射的能量分布实验曲线的数学表达式, 物理学家们从经典理论出发提出了**维恩公式**和**瑞利-金斯公式**. 前者在短波长处与实验结果符合程度好, 而在长波长处与实验结果符合程度差; 后者则恰好相反.

普朗克使用内插法结合两者, 提出了**普朗克公式**:

$$
M_0(\lambda, T)=2\pi hc^2\lambda^{-5}\frac{1}{\exp\left(\displaystyle\frac{hc}{\lambda kT}\right)-1}
$$

其中 $c$ 是真空中的光速, $k$ 是玻尔兹曼常数, $h$ 为普朗克常量:

$$
h=6.63\times10^{-34}\rm J\cdot s
$$

使用频率表达:

$$
M_0(\nu, T)=\frac{2\pi h\nu^3}{c^2}\frac{1}{\exp\left(\displaystyle\frac{h\nu}{kT}\right)-1}
$$

通过此公式可以推得维恩公式, 瑞利-金斯公式, 斯特藩-玻尔兹曼定律和维恩位移定律.

##### 普朗克能量子假设

普朗克使用数学方法得到上述公式后, 为解释其正确性, 放弃了一些经典理论观点: **辐射黑体是由大量带电的谐振子组成, 这些谐振子可以发射和吸收辐射能.** 并大胆假设, 这些谐振子的**能量不连续**, 而是**分立的**, 这些分立的能量只能是某一最小能量 $\varepsilon$ 的整数倍, 即 $n\varepsilon$ . 其中正整数 $n$ 称为**量子数**. 对于频率为 $\nu$ 的谐振子, 其最小能量为:

$$
\varepsilon=h\nu
$$

## 光电效应

### 实验规律

1. **饱和电流**: 单位时间内, 受光照的金属板释放出来的电子数和入射光的强度成正比.
2. **遏止电势差**: 遏制电势差与光强无关, 与光的频率成线性关系.
3. **截止频率(红限)**: 当入射光频率小于某个值时, 无光电效应发生.
4. **弛豫时间**: 从入射光开始照射直到金属放出电子, 无论光多微弱, 几乎是瞬时的, 弛豫时间不超过 $10^{-9} \rm s$ .

波动光学无法解释光电效应的实验规律.

### 爱因斯坦光子理论

受普朗克量子理论启发, 爱因斯坦提出光子理论: **光的能量是一份一份的, 想象光束是以光速 $c$ 运动的粒子流**, 这些粒子称为**光子**, 每一个光子的能量都是 $h\nu$ .

依据光子理论, 光电效应可以解释: 当金属中一个自由电子从入射光中吸收一个光子后, 就获得能量 $h\nu$ , 若 $h\nu$ 大于电子从金属表面溢出时所需的逸出功 $A$ , 这个电子就可以从金属中逸出. 爱因斯坦提出如下的**爱因斯坦光电效应方程**:

$$
h\nu=E_{k\text{m}}=\frac{1}{2}mv_\text{m}^2+A
$$

其中 $E_{k\text{m}}$ 是光电子的最大初动能. 若 $E_{k\text{m}}=0$ , 则得到红限:

$$
\nu_0=\frac{A}{h}
$$

### 光的波粒二象性

爱因斯坦的光子理论指出了光的**粒子性**:

光子**能量**: $E=h\nu=mc^2$

光子**质量**: $m=\displaystyle\frac{h\nu}{c^2}=\frac{h}{c\lambda}$

光子**动量**: $p=mc=\displaystyle\frac{h\nu}{c}=\frac{h}{\lambda}$

结合波动光学理论提出的光的**波动性**, 得到了**光的波粒二象性**.

## 康普顿效应

$X$ 射线源发射一束波长为 $\lambda_0$ 的 $X$ 射线, 投射到一块石墨上, 经石墨散射后, 散射束的波长及相对强度可以由晶体和探测器所组成的摄谱仪来测定. 康普顿发现, **散射光谱中除有与入射波波长 $\lambda_0$ 相同的射线外, 同时还存在 $\lambda>\lambda_0$ 的射线.**

### 光子理论的解释

康普顿效应可以使用光子理论解释:

认为这种现象是由光子与原子的外层与内层电子碰撞的结果.

光子与外层电子相碰撞时, 由于外层电子受原子核束缚较弱, 是近似自由的, 且由于其动能远小于光子能量, 因此是近似静止的. 故可以把外层电子看作是静止的自由电子.

考虑能量, 动量守恒:

$$
\left\{\begin{aligned}
    &h\nu_0+m_0c^2=h\nu+mc^2\\
    &\frac{h\nu_0}{c}=\frac{h\nu}{c}\cos\theta+mv\cos\phi\\
    &\frac{h\nu}{c}\sin\theta=mv\sin\phi
\end{aligned}\right.
$$

其中 $m=\displaystyle\frac{m_0}{\sqrt{1-(v/c)^2}}$ , 解得:

$$
\begin{aligned}
    \Delta \lambda&=\lambda-\lambda_0=\frac{h}{m_0c}(1-\cos\phi)=\frac{2h}{m_0c}\sin^2\frac{\phi}{2}\\
    \xRightarrow{\lambda_c:=h/(m_0c)}\Delta \lambda&=2\lambda_c\sin^2\frac{\phi}{2}
\end{aligned}
$$

由此可见 $\lambda>\lambda_0$ , 而且 $\Delta \lambda$ 与 $\lambda_0$ 无关. 其中 $\lambda_c$ 称为**康普顿波长**:

$$
\lambda_c:=\frac{h}{m_0c}=2.43\times10^{-12}\rm m
$$

电子的康普顿波长为 $0.00243\rm nm$ .

## 玻尔的氢原子理论

### 氢原子光谱

巴耳末公式:

$$
\nu=\frac{4c}{B}\left(\frac{1}{2^2}-\frac{1}{n^2}\right)
$$

或

$$
\frac{1}{\lambda}=\frac{4}{B}\left(\frac{1}{2^2}-\frac{1}{n^2}\right)
$$

其中 $B=364.56\rm nm$ , $n$ 为正整数. 代入 $n=3,4,5,\dotsm$ 时, 给出氢光谱中 $\rm H_\alpha,H_\beta, H_\gamma$ 的波长.

后来里德伯提出一个普遍方程:

$$
\frac{1}{\lambda}=R\left(\frac{1}{k^2}-\frac{1}{n^2}\right)\qquad
\begin{aligned}
    k&=1,2,3,\dotsm\\
    n&=k+1,k+2,k+3,\dotsm
\end{aligned}
$$

其中 $R=\displaystyle\frac{4}{B}=1.097\times10^7\rm m^{-1}$ 为里德伯常量.

$k$ 的取值 | 光谱系名称 | 波长区
:-: | :-: | :-:
$1$ | 莱曼系 | 紫外区
$2$ | 巴耳末系 | 可见光区
$3$ | 帕邢系 | 近红外区
$4$ | 布拉开系 | 远红外区

### 玻尔理论

为解释氢原子光谱, 玻尔提出三个基本假设:
1. 定态假设
   * 电子沿轨道作圆周运动
   * 不辐射电磁波
   * 定态能量不连续
2. 跃迁假设
   * 原子从一个定态跃迁到另一个定态, 会发射或吸收一个光子: $\nu=\displaystyle\frac{\vert \Delta E \vert}{h}$ .
3. 角动量量子化假设
   * 轨道角动量 $L=mvr=n\displaystyle\frac{h}{2\pi}\xlongequal{\hbar:=h/(2\pi)}n\hbar$

通过这三个假设, 可以得到以下结论.

**轨道半径量子化**: 第 $n$ 个定态的轨道半径为:

$$
r_n=n^2\left(\frac{\varepsilon_0h^2}{\pi me^2}\right)\xlongequal{a_0:=(\varepsilon_0h^2)/(\pi me^2)}n^2a_0
$$

其中 $a_0=0.0529\rm nm$ 为**玻尔半径**.

**能量量子化**: 处于第 $n$ 个定态的氢原子系统的能量为:

$$
E_n=-\frac{1}{n^2}\left(\frac{me^4}{8\varepsilon_0^2h^2}\right)\xlongequal{E_1:=-(me^4)/(8\varepsilon_0^2h^2)}\frac{E_1}{n^2}
$$

称这些量子化的能量为**能级**. 其中 $E_1=-13.6\rm eV$ , 这是氢原子的最低能级, 也称**基态能级**.

#### 玻尔理论的局限性

1. 不能解释稍复杂原子的光谱线规律;
2. 无法说明氢原子光谱线强度和线宽;
3. 其理论中混杂经典理论与量子假设.

## 微观粒子的波粒二象性与不确定性原理

### 德布罗意物质波

德布罗意提出, 质量为 $m$ 的粒子, 以速度 $v$ 匀速运动时, 具有能量 $E$ 和动量 $p$ ; 从波动性方面来看, 它具有波长 $\lambda$ 和频率 $\nu$ , 而这些量之间的关系符合:

$$
\left\{\begin{aligned}
    &E=mc^2=h\nu\\
    &p=mv=\frac{h}{\lambda}
\end{aligned}\right.
$$

所以对于具有静止质量 $m_0$ 的实物粒子来说, 若粒子具有速度 $v$ , 则粒子表现的平面单色波波长为:

$$
\lambda=\frac{h}{p}=\frac{h}{mv}=\frac{h}{m_0v}\sqrt{1-\left(\frac{v}{c}\right)^2}
$$

称此式为**德布罗意公式**. 人们通常把这种显示物质波动性的波称为**德布罗意波**, 也称**物质波**. 若 $v\ll c$ 则 $\displaystyle\lambda=\frac{h}{m_0v}$ .

### 波粒二象性

微观粒子不仅具有粒子性, 还具有波动性, 即**波粒二象性**. 粒子在某些条件下表现出粒子性, 某些条件下表现出波动性.

> 如同二维空间的生物考察三维空间的圆柱体时, 只使用二维空间的概念和语言, 是无法描述清楚圆柱体的: 某些实验中得到圆形, 某些实验中得到矩形. 最终得到其"方圆二象性".
> 
> 因此, **波粒二象性**这样模糊的描述在哲学上是自然的.

### 不确定性原理

海森伯提出微观粒子在坐标与动量两者不确定量之间的关系满足:

$$
\left\{\begin{aligned}
    \Delta x\Delta p_x\ge\frac{\hbar}{2}\\
    \Delta y\Delta p_y\ge\frac{\hbar}{2}\\
    \Delta z\Delta p_z\ge\frac{\hbar}{2}
\end{aligned}\right.
$$

称之为**海森伯坐标与动量的不确定关系**: 微观粒子不可能同时具有确定的坐标和相应的动量. 作数值估算时, 写作 $\Delta x\Delta p_x\ge \hbar$ 或 $\Delta x\Delta p_x\ge h$ 等形式.

此外, 还有微观粒子的能量与时间的不确定性关系:

$$
\Delta E\Delta t\ge\frac{\hbar}{2}
$$

> 若微观粒子处于某一状态范围对应的能量不确定度为 $\Delta E$ , 则微观粒子具有平均寿命 $\Delta t$ .

## 波函数与薛定谔方程

### 波函数及其统计诠释

描述自由粒子波动性的平面物质波的波函数为:

$$
\varPsi(x,t)=\varPsi_0\exp\left[-i \frac{2\pi}{h}(Et-px)\right]
$$

**在某一时刻, 空间某一地点, 粒子出现的概率正比于波函数与其共轭复数的乘积.** 即 $\vert\varPsi\vert^2=\varPsi\varPsi^*$ .

在空间中 $(x,y,z)$ 处找到粒子的概率就是:

$$
\vert\varPsi\vert^2\operatorname{d}V=\varPsi\varPsi^*\operatorname{d}V
$$

其中, $\vert\varPsi\vert^2=\varPsi\varPsi^*$ 表示在某一时刻在某点处单位体积内粒子出现的概率, 称为**概率密度**.

波函数模的平方作为概率密度, 有**归一化条件**:

$$
\iiint_\Omega\vert\varPsi\vert^2\operatorname{d}V=1
$$

还有**单值, 有限, 可导, 一阶导数连续**的特性.

### 薛定谔方程

薛定谔方程是量子力学中的基本方程之一, 不可由公式推导.

#### 自由粒子的薛定谔方程

质量为 $m$ 的自由粒子的波函数满足:

$$
-\frac{\hbar^2}{2m}\nabla^2\varPsi=i\hbar\frac{\partial \varPsi}{\partial t}
$$

#### 势场中粒子的薛定谔方程

质量为 $m$ 的粒子在势能为 $U(\vec{r},t)$ 的势场中的满足:

$$
-\frac{\hbar^2}{2m}\nabla^2\varPsi+U\varPsi=i\hbar\frac{\partial \varPsi}{\partial t}
$$

采用哈密顿算符: $\hat{H}=\displaystyle-\frac{\hbar^2}{2m}\nabla^2+U$ , 则该方程可以写作:

$$
\hat{H}\varPsi=i\hbar\frac{\partial \varPsi}{\partial t}
$$

> 算符是一种操作, 物理中每一个物理量都对应一个算符. 以下给出一些算符:
> 1. 位置算符就是位置本身;
> 2. 能量算符: $E\rightarrow\displaystyle i\hbar\frac{\partial}{\partial t}$ 或 $E\rightarrow \hat{H}$ ;
> 3. 动量算符: $\vec{p}\rightarrow -i\hbar\nabla$ .
> 
> 若要求算符为 $\hat{A}$ 的物理量的均值, 则可以使用积分 $\displaystyle\int_\Omega \varPsi^*\hat{A}\varPsi\operatorname{d}\omega$ 来计算.

##### 定态薛定谔方程

若势能只是坐标的函数而与时间无关, 则可以把波函数分离变量: $\varPsi(\vec{r},t)=\psi(\vec{r})\cdot f(t)$ . 代入上述方程可解得:

$$
E:=i\hbar\frac{\operatorname{d}f(t)}{\operatorname{d}t}\frac{1}{f(t)}\Rightarrow f(t)=\exp\left(-\frac{i}{h}Et\right)
$$

其中, $E$ 就是粒子的能量. 同时得到**定态薛定谔方程**:

$$
-\frac{\hbar^2}{2m}\nabla^2\psi+U\psi=E\psi
$$

或

$$
\nabla^2\psi+\frac{2m}{\hbar^2}(E-U)\psi=0
$$

易得: $\vert\varPsi\vert^2=\vert\psi\vert^2$ .

### 一维无限深势阱

设一维无限深势阱的势能分布:

$$
U(x)=\left\{\begin{aligned}
    0,&&&0<x<a\\
    +\infty,&&&x\le0,x\ge a
\end{aligned}\right.
$$

阱外, 薛定谔方程的解为 $\psi_e(x)\equiv0$ 说明粒子不可能出现在阱外.

阱内, 薛定谔方程的解为 $\psi_i(x)=\displaystyle\sqrt{\frac{2}{a}}\sin\frac{n\pi}{a}x\quad(n=1,2,3,\dotsm)$ .

综合得到定态波函数:

$$
\psi(x)=\left\{\begin{aligned}
    \sqrt{\frac{2}{a}}\sin\frac{n\pi}{a}x,&&&0<x<a\\
    0,&&&x\le0,x\ge a
\end{aligned}\right.
$$

一维无限深势阱中的粒子运动特征如下:
1. 粒子的能量不连续, 是量子化的:
   
   $$
   E_n=\frac{\pi^2\hbar^2}{2ma^2}n^2(n=1,2,3,\dotsm)
   $$

2. 粒子的最小能量不为零:
   
   $$
   E_\text{min}=E_1=\frac{\pi^2\hbar^2}{2ma^2}
   $$

3. 粒子出现的概率随位置变化;
4. 粒子的物质波在阱内形成驻波.

### 一维有限势垒

设一维有限势垒的势能分布:

$$
U(x)=\left\{\begin{aligned}
    U_0,&&&0<x<a\\
    0,&&&x\le0,x\ge a
\end{aligned}\right.
$$

有结论:
1. **隧道效应**: 粒子能够穿透比其动能更高的势垒的现象;
2. **反射系数**: 粒子反射的"强度"与入射的"强度"之比("强度"指波函数模的平方):
   
   $$
   R=\frac{(k_1^2-k_2^2)\sin^2k_2a}{(k_1^2-k_2^2)\sin^2k_2a+4k_1^2k_2^2}
   $$
   
   尽管 $E>U_0$ , 也有 $R\neq0$ . 这说明粒子仍有一定概率反射;
3. **透射系数**: 粒子透射的"强度"与入射的"强度"之比:
   
   $$
   T=\frac{4k_1^2k_2^2}{(k_1^2-k_2^2)\sin^2k_2a+4k_1^2k_2^2}
   $$
   
   尽管 $E<U_0$ , 也有 $T\neq0$ . 这说明粒子仍有一定概率透射.

## 量子力学中的氢原子

### 能量量子化

氢原子的能量为:

$$
E_n=-\frac{1}{n^2}\left(\frac{me^4}{8\varepsilon_0^2h^2}\right)\xlongequal{E_1:=-(me^4)/(8\varepsilon_0^2h^2)}\frac{E_1}{n^2}\quad n=1,2,3,\dotsm
$$

其中 $E_1=-13.6\rm eV$ , 而 $n$ 称为**主量子数**.

### 角动量大小量子化

氢原子电子绕核转动的角动量大小:

$$
L=\sqrt{l(l+1)}\hbar\quad l=0,1,2,\dotsm,n-1
$$

这与玻尔理论的结果不同, 实验表明, 量子力学理论的结果是正确的. 称 $l$ 为**角量子数(副量子数)**.

### 角动量空间量子化

氢原子电子绕核转动的角动量方向也不能连续改变. 角动量在外磁场方向 $Z$ 的投影:

$$
L_z=m_lh\quad m_l=0,\pm1,\pm2,\dotsm,\pm l
$$

称 $m_l$ 为**磁量子数**.

#### 塞曼效应

光源处于磁场中时, 一条谱线会分裂为若干条谱线. 量子力学对此的解释为: 原子中绕核运动的电子不仅有角动量, 还有磁矩 $\mu_z$ .

$$
\mu_z=-m_l\frac{e\hbar}{2m_e}=-m_l\mu_\text{B}
$$

其中 $\mu_\text{B}$ 为**玻尔磁子**.

由于磁场的作用, 粒子附加的能量为 $\Delta E=-\vec{\mu}\cdot\vec{B}=-\mu_zB=m_l\mu_\text{B}B$ .

### 电子自旋

电子除了轨道运动之外, 还存在一种"自旋运动", 具有**自旋角动量 $\vec S$** 和 **自旋磁矩 $\vec \mu_s$** , 它们都只能取两个特定值:

$$
\begin{aligned}
    S&=\sqrt{s(s+1)}\hbar&&s=\frac{1}{2}\\
    S_z&=m_s\hbar&&m_s=\pm\frac{1}{2}\\
    \mu_{sz}&=\pm\mu_\text{B}
\end{aligned}
$$

称 $m_s$ 为**自旋磁量子数**.

量子物理中用以上四个量子数可以**完全表征**电子的运动状态:
1. 主量子数 $n=1,2,3,\dotsm$ , 大体上决定了电子的能量;
2. 副量子数 $l=0,1,2,\dotsm,n-1$ , 决定了电子的轨道角动量大小, 对能量有些许影响;
3. 磁量子数 $m_l=0,\pm1,\pm2,\dotsm,\pm l$ , 有外磁场时决定了电子轨道角动量的空间取向;
4. 自旋磁量子数 $m_s=\displaystyle\pm\frac{1}{2}$ , 决定了电子自旋角动量空间取向.

### 电子云

量子力学中没有轨道的概念, 而是以**电子云**的概念描述电子的运动. 电子出现在某处的概率就使用概率密度 $\vert\varPsi\vert^2$ 来计算.

## 原子的电子壳层结构

引入**壳层**: 主量子数 $n$ 不同的电子分布在不同的壳层上.

$n$ | 1 | 2 | 3 | 4 | 5 | 6 
:-: |:-:|:-:|:-:|:-:|:-:|:-:
符号 | $\rm K$ | $\rm L$ | $\rm M$ | $\rm N$ | $\rm O$ | $\rm P$

主量子数 $l$ 相同而角量子数不同的电子, 分布在不同的支壳层上:

$l$ | 0 | 1 | 2 | 3 | 4 | 5 
:-: |:-:|:-:|:-:|:-:|:-:|:-:
符号 | $\rm s$ | $\rm p$ | $\rm d$ | $\rm f$ | $\rm g$ | $\rm h$

### 泡利不相容原理

在一个原子中, 不能有两个或两个以上的电子处于完全相同的量子态, 即他们不可能具有一组完全相同的量子数.

可以推得, 第 $n$ 个壳层容纳电子的最大数目为 $Z_n=\displaystyle\sum_{l=0}^{n-1}2(2l+1)=2n^2$ .

### 能量最低原理

原子处于正常状态时, 每个电子都趋于占据可能的最低能级.
