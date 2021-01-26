---	
layout:     post	
title:      『Physics』 Basis of Thermodynamics	
subtitle:   『物理学』 热力学基础    
date:       2020-12-29	   
author:     Coekjan 
header-img: img/post-bg-PHY.jpg	
catalog:    true	
katex:    true    
tags:	
    - Physics  
---

## 热力学第零定律与三大定律

### 热力学第零定律

**若两个物体都与处于确定状态的第三物体处于热平衡，则这两个物体彼此处于热平衡。**

温度是决定一个物体是否与其他物体处于热平衡的宏观性质。

### 热力学第一定律

一般情况下，当系统状态发生变化时，做功与热传递往往同时存在。若有一系统，外界对它传递热量为 $Q$ , 系统内能变化 $\Delta E = E_2 - E_1$ , 同时**系统对外做功** $A$ , 则无论过程如何，总有：

$$
Q = \Delta E + A = E_2 - E_1 + A
$$

对于一个微小的变化过程，可写作：

$$
\operatorname{\delta} Q = \operatorname{d}E + \operatorname{\delta} A
$$

> 气体对外做功，可以由下式计算：
> 
> $$
> \operatorname{d}A=p\operatorname{d}V
> $$

当气体经历准静态过程时，考虑气体做功的计算公式，可以写作：

$$
Q = \Delta E + \int_{V_1}^{V_2}p\operatorname{d}V
$$

### 热力学第二定律

两种表述：

1. 克劳修斯表述：热量可以自发地从温度高的物体传递到温度低的物体，但不可能自发地从温度低的物体传递到温度高的物体。
2. 开尔文-普朗克表述：不可能从单一热源吸收热量，并将这热量完全变为功，而不产生其他影响。

### 热力学第三定律

绝对零度 ( $T=0\rm K=-273.15^{\circ}C$ ) 不可达到。

## 理想气体的准静态过程

### 等体过程与气体的摩尔定容热容

等体过程中，$\operatorname{d}V=0$ , 因此 $\operatorname{\delta}A=0$ , 根据热力学第一定律，可知：

$$
\operatorname{\delta}Q_V=\operatorname{d}E
$$

对于有限的变化：

$$
Q_V=E_2-E_1
$$

引入摩尔定容热容 $C_{V,m}$ , 表示 $1\rm mol$ 气体在体积不变时，温度改变 $1\rm K$ 所吸收或放出的热量。

由此，质量为 $m$ 的气体在等体过程中，温度改变 $\operatorname{d}T$ 所需的热量为：

$$
\operatorname{\delta}Q_V=\frac{m}{M}C_{V,m}\operatorname{d}T
$$

考虑等体过程中 $\operatorname{\delta}Q_V=\operatorname{d}E$ , 因此可以写作：

$$
\operatorname{d}E=\frac{m}{M}C_{V,m}\operatorname{d}T
$$

**事实上此式通用于理想气体的内能计算，不限于等体过程**.

按气体动理论，理想气体的内能有计算式：$E=\displaystyle\frac{m}{M}\frac{i}{2}RT$

因此，有：$\operatorname{d}E=\displaystyle\frac{m}{M}\frac{i}{2}R\operatorname{d}T$

与上面的结果比较，可得到摩尔定容热容的计算式：

$$
C_{V,m}=\frac{i}{2}R
$$

这说明**摩尔定容热容仅与分子自由度**有关。

### 等压过程与气体的摩尔定压热容

等压过程中，$\operatorname{d}p=0$ .

下面考虑计算气体体积增加 $\operatorname{d}V$ 时的做功 $\operatorname{\delta}A$ . 根据理想气体物态方程：$pV=\displaystyle\frac{m}{M}RT$ , 有：

$$
\operatorname{\delta}A=p\operatorname{d}V=\frac{m}{M}R\operatorname{d}T
$$

根据热力学第一定律，系统吸热：

$$
\operatorname{\delta}Q_p=\operatorname{d}E+\frac{m}{M}R\operatorname{d}T
$$

全过程中，计算做功：

$$
A=p(V_2-V_1)=\frac{m}{M}R(T_2-T_1)
$$

因此全过程中吸热：

$$
Q_p=E_2-E_1+p(V_2-V_1)=E_2-E_1+\frac{m}{M}R(T_2-T_1)
$$

引入摩尔定压热容 $C_{p,m}$ , 表示 $1\rm mol$ 气体在压强不变时，温度改变 $1\rm K$ 所需的热量。

则有：

$$
\operatorname{\delta}Q_p=\frac{m}{M}C_{p,m}\operatorname{d}T
$$

考虑到 $\operatorname{d}E=\displaystyle\frac{m}{M}\frac{i}{2}R\operatorname{d}T$

因此：

$$
\operatorname{\delta}Q_p=\frac{m}{M}\frac{i}{2}R\operatorname{d}T+\frac{m}{M}R\operatorname{d}T=\frac{m}{M}\left(\frac{i}{2}+1\right)R\operatorname{d}T
$$

与上面的结果比较，得到摩尔定压热容的计算式：

$$
C_{p,m}=\frac{i+2}{2}R
$$

比较摩尔定容热容和摩尔定压热容的计算式，容易得到**迈耶公式**:

$$
C_{p,m}=C_{V,m}+R
$$

以及热容比：

$$
\gamma=\frac{C_{p,m}}{C_{V,m}}=\frac{i+2}{i}
$$

### 等温过程

等温过程中，$\operatorname{d}T=0$ , 由于理想气体的内能只与温度有关，因此该过程中，$\operatorname{d}E=0$ .

等温过程中，气体对外做功：

$$
\begin{aligned}
    A&=\int_{V_1}^{V_2}p\operatorname{d}V=\int_{V_1}^{V_2}\frac{m}{M}RT\cdot\frac{\operatorname{d}V}{V}\\
    &=\frac{m}{M}RT\ln\frac{V_2}{V_1}\\
    &=\frac{m}{M}RT\ln\frac{p_1}{p_2}
\end{aligned}
$$

根据热力学第一定律：

$$
\operatorname{\delta}Q_T=A=\frac{m}{M}RT\ln\frac{V_2}{V_1}=\frac{m}{M}RT\ln\frac{p_1}{p_2}
$$

### 绝热过程

绝热过程中，$\operatorname{\delta}Q=0$ , 根据该特征，应用热力学第一定律，得到：

$$
\operatorname{d}E+p\operatorname{d}V=0\Leftrightarrow\operatorname{\delta}A=-\operatorname{d}E
$$

因而：

$$
A=-(E_2-E_1)=-\frac{m}{M}C_{V,m}(T_2-T_1)
$$

在绝热过程中，$p$ , $V$ , $T$ , 同时变化，他们两两之间的关系称为**绝热过程方程**, 下面进行推导。

首先，根据上述推导，我们得到：

$$
p\operatorname{d}V=-\frac{m}{M}C_{V,m}\operatorname{d}T
$$

理想气体的状态方程与上式形式相似，考虑对状态方程微分：

$$
pV=\frac{m}{M}RT
\Rightarrow
p\operatorname{d}V+V\operatorname{d}p=\frac{m}{M}R\operatorname{d}T
$$

由上述两式，消去 $\operatorname{d}T$ , 可得：

$$
C_{V,m}(p\operatorname{d}V+V\operatorname{d}p)=-Rp\operatorname{d}V
$$

由迈耶公式，$R=C_{p,m}-C_{V,m}$ , 简化上式，即得：

$$
C_{V,m}V\operatorname{d}p+C_{p,m}p\operatorname{d}V=0
$$

考虑 $\displaystyle\gamma=\frac{C_{p,m}}{C_{V,m}}$ , 上式化为：

$$
\frac{\operatorname{d}p}{p}+\gamma\frac{\operatorname{d}V}{V}=0
$$

积分即得：

$$
pV^\gamma\equiv\rm Const
$$

应用 $pV=\displaystyle\frac{m}{M}RT$ , 可得一系列方程：

$$
\begin{aligned}
    &pV^\gamma\equiv\rm Const\\
    &V^{\gamma-1}T\equiv\rm Const\\
    &p^{\gamma-1}T^{-\gamma}\equiv\rm Const
\end{aligned}
$$

这就是**绝热过程方程**.

#### 绝热过程中的做功

考虑使用 $C_{V,m}=\displaystyle\frac{i}{2}R$ 代换 $A=\displaystyle-\frac{m}{M}C_{V,m}(T_2-T_1)$ 中的 $C_{V,m}$ .

得到：

$$
A=\frac{i}{2}\frac{m}{M}R(T_1-T_2)
$$

考虑理想气体物态方程，代入得：

$$
A=\frac{i}{2}(p_1V_1-p_2V_2)\xlongequal[\Rightarrow i=2/(\gamma-1)]{\gamma:=(i+2)/i}\frac{p_1V_1-p_2V_2}{\gamma-1}
$$

### 多方过程

事实上，气体进行的过程是多方过程，是介于绝热过程与等温过程间的过程，过程方程可写为：

$$
pV^n\equiv\rm Const
$$

其中 $n$ 称为多方指数 （一般而言 $1<n<\gamma$ ):
1. 当 $n=0$ 时，就是等压过程；
2. 当 $n\rightarrow\infty$ 时，就是等容过程；
3. 当 $n=1$ 时，就是等温过程；
4. 当 $n=\gamma$ 时，就是绝热过程。

多方过程中的做功可以由绝热过程的做功方程推广而来：

$$
A=\frac{p_1V_1-p_2V_2}{n-1}
$$

## 循环过程与卡诺循环

一个热力学系统从某一状态出发，经过一系列变化过程，最终回到初始状态，这样的过程称为**循环过程**. 若一个循环过程所经历的每一个分过程都是准静态过程，那么循环过程就可以在 $p-V$ 图上用一闭合曲线表示。若系统沿闭合曲线顺时针方向循环，则称为**正循环**, 反之称为**逆循环**.

循环过程中，系统经过一次循环内能不变，根据热力学第一定律有：

$$
\Delta E=0\Rightarrow Q=A
$$

工程上，正循环设备称为**热机**, $A>0$ ; 逆循环设备称为**制冷机**, $A<0$ .

### 热机

热机的工作过程就是工质从高温热源吸收热量 $Q_1$ , 其中一部分热量 $Q_2$ 传给低温热源，同时工质对外做功 $A$ .

热机循环的一个重要性能指标是**热机效率**, 用 $\eta$ 表示：

$$
\eta=\frac{A}{Q_1}=\frac{Q_1-Q_2}{Q_1}=1-\frac{Q_2}{Q_1}
$$

### 制冷机

制冷机的工作过程就是外界对工质做功 $A$ 与从低温热源吸收热量 $Q_2$ 全部以热能形式转移给高温热源，$Q_1=A+Q_2$ .

制冷机循环的一个重要性能指标是**制冷系数**, 用 $w$ 表示。

$$
w=\frac{Q_2}{A}=\frac{Q_2}{Q_1-Q_2}
$$

### 卡诺循环

卡诺循环是两个温度恒定热源间工作的循环过程，是通过**绝热膨胀-等温膨胀-绝热压缩-等温压缩**四个过程组成的循环过程，理论上具有最高效率。

通过等温过程与绝热过程的方程，可以推得：

$$
\frac{Q_1}{T_1}=\frac{Q_2}{T_2}
$$

卡诺热机的效率：

$$
\eta_c=1-\frac{Q_2}{Q_1}=1-\frac{T_2}{T_1}
$$

可见：
1. 要完成一次卡诺循环，需要有高温，低温两个热源；
2. 卡诺循环效率仅与两热源的温度有关，高温热源温度越高，低温热源温度越低，卡诺热机效率越大，也就是两热源温差越大，效率越高；
3. 卡诺热机效率总小于 1( $T_2\neq 0$ ).

卡诺制冷机的制冷系数：

$$
w_c=\frac{Q_2}{Q_1-Q_2}=\frac{T_2}{T_1-T_2}
$$

## 可逆过程，不可逆过程与卡诺定理

### 可逆过程与不可逆过程

设有一过程，使物体从状态 $A$ 变化到状态 $B$ . 若存在另一过程，不仅使得物体从状态 $B$ 恢复到状态 $A$ , **且不引起其他变化**, 则称从状态 $A$ 到状态 $B$ 的过程是**可逆过程**; 若不存在，则称从状态 $A$ 到状态 $B$ 的过程是**不可逆过程**.

热力学中，只有过程进行得无限缓慢，没有由于摩擦等引起机械能的耗散，由一系列无限接近于平衡状态的中间状态所组成的准静态过程，才是可逆过程。

### 卡诺定理

从热力学第二定律可得：
1. 在同样高低温热源（高温热源温度为 $T_1$ , 低温热源温度为 $T_2$ ) 之间工作的一切可逆机，无论用什么工作物，效率都是 $\displaystyle\left(1-\frac{T_2}{T_1}\right)$ .
2. 在同样高低温热源（高温热源温度为 $T_1$ , 低温热源温度为 $T_2$ ) 之间工作的一切不可逆机，无论用什么工作物，效率都小于 $\displaystyle\left(1-\frac{T_2}{T_1}\right)$ .

综合：

$$
\eta\le1-\frac{T_2}{T_1}
$$

当且仅当工作的是可逆机时，取等号。

## 熵和玻尔兹曼关系

### 熵

卡诺热机中，改用 $Q_{1,2}$ 表示**吸收**的热量：

$$
-\frac{Q_1}{Q_2}=\frac{T_1}{T_2}
$$

可见：

$$
\frac{Q_1}{T_1}+\frac{Q_2}{T_2}=0
$$

若一可逆过程由卡诺循环组成，则有：

$$
\sum\frac{Q}{T}=0
$$

事实上，任意**可逆循环**中，都可以近似看作许多卡诺循环组成，用于近似的卡诺循环越多，近似效果越好，求和变为积分：

$$
\oint\frac{\operatorname{\delta}Q}{T}=0
$$

环路积分恒为 $0$ , 意味着该路径上任意两点间的路径积分与路径无关。定义熵增：

$$
\Delta S=S_2-S_1=\int_1^2\frac{\operatorname{\delta}Q}{T}
$$

也可写作微分形式：

$$
\operatorname{\delta}S=\frac{\operatorname{\delta}Q}{T}
$$

**可逆过程中**, 把 $\displaystyle\frac{\operatorname{\delta}Q}{T}$ 看作系统的熵变；**可逆循环中**, 系统熵变为 $0$ .

### 玻尔兹曼关系

引入**热力学概率** $W$ , 表示宏观系统状态包含的微观状态数。波尔兹曼给出关系：

$$
S=k\ln W
$$

其中，$k$ 是玻尔兹曼常数。熵的这个定义表明它是分子热运动无序性或混乱性的量度。

## 熵增原理

称一个系统为**封闭系统**, 是说其与外界没有能量交换。在封闭系统中发生的**任何不可逆过程，都导致了整个系统的熵增加**, 系统的总熵只在可逆过程中不变。

这也称作**热力学第二定律的统计意义**.