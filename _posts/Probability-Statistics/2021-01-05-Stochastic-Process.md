---	
layout:     post	
title:      『Probability Statistics』 Stochastic Process	
subtitle:   『概率统计』 随机过程    
date:       2021-01-05	   
author:     Coekjan 
header-img: img/post-bg-PS.jpg	
catalog:    true	
katex:    true    
tags:	
    - Probability Statistics  
---

## 随机过程的基本概念

### 随机过程的概率分布

#### 随机过程的 $n$ 维分布函数

设 $\{X(t),t\in T\}$ 是一随机过程, 对于参数集 $T$ 内任意 $n$ 个元素 $t_1,t_2,\dotsm,t_n$ 所对应的 $n$ 个随机变量

$$
X(t_1),X(t_2),\dotsm,X(t_n)
$$

的联合分布

$$
F(x_1,\dotsm,x_n;t_1,\dotsm,t_n)=P(X(t_1)\le x_1,\dotsm,X(t_n)\le x_n)
$$

称为随机过程 $X(t)$ 的 $n$ 维分布函数.

对于状态连续型的随机过程, 有 $n$ 维密度函数:

$$
f(x_1,\dotsm,x_n;t_1,\dotsm,t_n)
$$

对于状态离散型的随机过程, 有 $n$ 维联合分布律:

$$
P(x_1,\dotsm,x_n;t_1,\dotsm,t_n)
$$

一般来说, 随机过程的所有分布函数(或密度函数, 分布律)完全确定了随机过程的统计特征.

#### 独立过程

若对任何正整数 $n$ , 随机过程的任意 $n$ 个状态都是相互独立的, 则称此过程为独立过程. 此时有:

$$
F(x_1,\dotsm,x_n;t_1,\dotsm,t_n)=\prod_{i=1}^nF(x_i;t_i)
$$

#### 两个随机过程的有限维联合分布及独立性

设 $\{X(t),t\in T_1\}$ 和 $\{Y(t),t\in T_2\}$ 是两个随机过程, 由过程 $X(t)$ 的任意 $m$ 个状态: $X(t_1),X(t_2),\dotsm,X(t_m)$ , 和过程 $Y(t)$ 的任意 $n$ 个状态: $Y(t_1'),Y(t_2'),\dotsm,Y(t_n')$ 组成 $m+n$ 维随机向量. 其分布函数

$$
F_{XY}(x_1,\dotsm,x_m,y_1,\dotsm,y_n;t_1,\dotsm,t_m,t_1',\dotsm,t_n')
$$

称为随机过程 $X(t)$ 和 $Y(t)$ 的 $m+n$ 维联合分布函数.

若对于任意的正整数 $m,n$ , 对于 $T_1$ 中任意数组 $t_1,\dotsm,t_m$ 和 $T_2$ 中任意数组 $t_1',\dotsm,t_n'$ , 关系式

$$
\begin{aligned}
    &F_{XY}(x_1,\dotsm,x_m,y_1,\dotsm,y_n;t_1,\dotsm,t_m,t_1',\dotsm,t_n')=\\
    &F_X(x_1,\dotsm,x_m;t_1,\dotsm,t_m)\cdot F_Y(y_1,\dotsm,y_n;t_1',\dotsm,t_n')
\end{aligned}
$$

则称这两个过程相互独立.

### 随机过程的数字特征

考虑随机过程 $\{X(t),t\in T\}$ , 对于任意给定的 $t\in T$ , 过程在 $t$ 的状态 $X(t)$ 是一个随机变量, 则:
1. 过程在 $t$ 的状态 $X(t)$ 的数学期望为
   
   $$
   \mu_X(t)=E[X(t)]
   $$

   对于一切 $t\in T$ , $\mu_X(t)$ 是 $t$ 的函数, 称为随机过程 $X(t)$ 的均值函数, 简称均值.
2. 过程在 $t$ 的状态 $X(t)$ 的二阶原点矩
   
   $$
   \varPsi_X^2(t)=E[X^2(t)]
   $$

   称为随机过程 $X(t)$ 的均方值函数, 简称均方值.
3. 二阶中心矩
   
   $$
   \sigma_X^2(t)=D[X(t)]=E[X(t)-\mu_X(t)]^2=E[X^2(t)]-\mu_X^2(t)
   $$

   称为随机过程 $X(t)$ 的方差函数, 简称方差, 均方差为 $\sigma_X(t)$ .
4. 任选 $t_1,t_2\in T$ , 状态 $X(t_1),X(t_2)$ 是两个随机变量,
   
   $$
   R_X(t_1,t_2)=E[X(t_1)\cdot X(t_2)]
   $$

   称为随机过程 $X(t)$ 的自相关函数, 简称相关函数.
5. 任选 $t_1,t_2\in T$ , 状态 $X(t_1),X(t_2)$ 是两个随机变量,
   
   $$
   C_X(t_1,t_2)=E\left\{ [X(t_1)-\mu_X(t_1)]\cdot [X(t_2)-\mu_X(t_2)] \right\}
   $$

   称为随机过程 $X(t)$ 的自协方差函数, 简称协方差函数.

这五个数字特征具有如下关系:

$$
\begin{aligned}
    \varPsi_X^2(t)&=R_X(t,t)\\
    C_X(t_1,t_2)&=R_X(t_1,t_2)-\mu_X(t_1)\cdot\mu_X(t_2)\\
    \sigma_X^2(t)&=C_X(t,t)\\
    &=R_X(t,t)-\mu_X^2(t)\\
    &=\varPsi_X^2(t)-\mu_X^2(t)
\end{aligned}
$$

#### 连续型随机过程的数字特征

对于连续型随机过程 $X(t)$ , 设一维概率密度为 $f_1(x_1;t)$ , 则有:
1. $\mu_X(t)=\displaystyle\int_{-\infty}^{+\infty}xf_1(x;t)\operatorname{d}x$ ;
2. $\varPsi_X^2(t)=\displaystyle\int_{-\infty}^{+\infty}x^2f_1(x;t)\operatorname{d}x$ ;
3. $R_X(t_1,t_2)=\displaystyle\int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty}x_1x_2f_2(x_1,x_2;t_1,t_2)\operatorname{d}x_1\operatorname{d}x_2$

#### 两个随机过程的互相关函数

对于两个随机过程 $\{X(t),t\in T_1\}$ 和 $\{Y(t),t\in T_2\}$ , 任选 $t_1\in T_1,t_2\in T_2$ , 对应有过程 $X(t)$ 在 $t_1$ 的状态 $X(t_1)$ 和过程 $Y(t)$ 在 $t_2$ 的状态 $Y(t_2)$ . $X(t_1)$ 和 $Y(t_2)$ 的二阶原点混合矩

$$
R_{XY}(t_1,t_2)=E[X(t_1)\cdot Y(t_2)]
$$

称为随机过程 $X(t)$ 和 $Y(t)$ 的互相关函数.

$X(t_1)$ 和 $Y(t_2)$ 的二阶中心混合矩

$$
C_{XY}(t_1,t_2)=E\{[X(t_1)-\mu_X(t_1)]\cdot[Y(t_2)-\mu_Y(t_2)]\}
$$

称为随机过程 $X(t)$ 和 $Y(t)$ 的互协方差函数. 显然有:

$$
C_{XY}(t_1,t_2)=R_{XY}(t_1,t_2)-\mu_X(t_1)\cdot\mu_Y(t_2)
$$

称随机过程 $X(t)$ 与 $Y(t)$ 不相关, 是指对于任意 $t_1\in T_1,t_2\in T_2$ , 都有 $C_{XY}(t_1,t_2)=0$ .

> 此处的不相关是指不线性相关. 因此不相关的随机过程, 不一定独立.

## 平稳过程

### 严平稳过程

设随机过程 $\{X(t),t\in T\}$ , 如果对任意 $t_1,t_2,\dotsm,t_n\in T$ , 任意实数 $\varepsilon$ , 有 $t_1+\varepsilon,t_2+\varepsilon,\dotsm,t_n+\varepsilon\in T$ , 对任意 $n$ 维分布函数都有

$$
\begin{aligned}
    &F(x_1,x_2,\dotsm,x_n;t_1,t_2,\dotsm,t_n)\\
    =&F(x_1,x_2,\dotsm,x_n;t_1+\varepsilon,t_2+\varepsilon,\dotsm,t_n+\varepsilon)\qquad n=1,2,\dotsm
\end{aligned}
$$

则称 $X(t)$ 为严平稳过程, 或狭义平稳过程.

#### 严平稳过程一维分布和二维分布的性质

设随机过程 $\{X(t),t\in T\}$ 是严平稳过程, 则:
1. 一维分布函数
   
   $$
   F_1(x_1;t_1)=F_1(x_1)
   $$
   
   不依赖于参数 $t$ .
2. 二维分布函数
   
   $$
   F_2(x_1,x_2;t_1,t_2)=F_2(x_1,x_2;\tau)\quad (\tau=t_2-t_1)
   $$

   仅依赖于参数间距, 而与参数本身无关.

#### 离散状态随机过程的严平稳条件

设离散状态随机过程 $\{X(t),t\in T\}$ . 对于任意 $t_1,t_2,\dotsm,t_n\in T$ , 任意实数 $\varepsilon$ , 有 $t_1+\varepsilon,t_2+\varepsilon,\dotsm,t_n+\varepsilon\in T$ , 且有

$$
\begin{aligned}
    &P(X(t_1)=x_1,X(t_2)=x_2,\dotsm,X(t_n)=x_n)\\
    =&P(X(t_1+\varepsilon)=x_1,X(t_2+\varepsilon)=x_2,\dotsm,X(t_n+\varepsilon)=x_n)
\end{aligned}
$$

#### 连续状态随机过程的严平稳条件

设连续状态随机过程 $\{X(t),t\in T\}$ . 对于任意 $t_1,t_2,\dotsm,t_n\in T$ , 任意实数 $\varepsilon$ , 有 $t_1+\varepsilon,t_2+\varepsilon,\dotsm,t_n+\varepsilon\in T$ , 且有

$$
\begin{aligned}
    &f(x_1,x_2,\dotsm,x_n;t_1,t_2,\dotsm,t_n)\\
    =&f(x_1,x_2,\dotsm,x_n;t_1+\varepsilon,t_2+\varepsilon,\dotsm,t_n+\varepsilon)
\end{aligned}
$$

一维概率密度

$$
f_1(x_1;t_1)=f_1(x_1)
$$

二维概率密度

$$
f_2(x_1,x_2;t_1,t_2)=f_2(x_1,x_2;\tau)\quad(\tau=t_2-t_1)
$$

#### 严平稳过程的数字特征性质

设随机过程 $\{X(t),t\in T\}$ 是严平稳过程, 若该过程的二阶矩存在, 则
1. $\mu_X(t)=\mu_X,\varPsi_X^2(t)=\varPsi_X^2,\sigma_X^2(t)=\sigma_X^2$ 均为常数, 与参数 $t$ 无关.
2. $R_X(t_1,t_2)=R_X(\tau),C_X(t_1,t_2)=C_X(\tau)$ 仅依赖于参数间距, 而不依赖于参数本身.

### 广义平稳过程

设随机过程 $\{X(t),t\in T\}$ , 对于任意 $t\in T$ , 满足:
1. $E[X^2(t)]$ 存在且有限;
2. $E[X(t)]=\mu_X$ 是常数;
3. $E[X(t)X(t+\tau)]=R_X(\tau)$ 仅依赖于 $\tau$ 而与 $t$ 无关.

则称 $X(t)$ 为广义平稳过程, 或称宽平稳过程, 简称平稳过程.

> 严平稳过程是广义平稳过程的前提是: 二阶矩存在.

#### 广义平稳过程的数字特征性质

设随机过程 $\{X(t),t\in T\}$ 是平稳过程, 则
1. $E[X(t)]=\mu_X$ 是常数;
2. $E[X^2(t)]=\varPsi_X^2$ 是常数;
3. $D[X(t)]=\varPsi_X^2-\mu_X^2=\sigma_X^2$ 是常数;
4. $E[X(t)X(t+\tau)]=R_X(\tau)$ 仅依赖于 $\tau$ 而与 $t$ 无关;
5. $C_X(t,t+\tau)=R_X(\tau)-\mu_X^2=C_X(\tau)$ 仅依赖于 $\tau$ 而与 $t$ 无关.

#### 两个平稳过程的关系

设 $X(t)$ 和 $Y(t)$ 是两个平稳过程, 如果互相关函数

$$
E[X(t)Y(t+\tau)]=R_{XY}(\tau)
$$

仅是参数间距的函数, 则称 $X(t)$ 和 $Y(t)$ 平稳相关, 或联合平稳. 此时

$$
C_{XY}(\tau)=\operatorname{Cov}(X(t),Y(t+\tau))=R_{XY}(\tau)-\mu_X\mu_Y
$$

另, 称

$$
\rho_{XY}(\tau)=\frac{C_{XY}(\tau)}{\sqrt{C_X\cdot C_Y}}
$$

为标准互协方差函数.

## 马尔可夫链引论

### 马尔可夫链的概念

设随机过程 $\{X(t),t\in T\}$ 的状态空间 $S$ 是有限集或可列集, 若对任意正整数 $n$ , 对于 $T$ 内任意 $n+1$ 个参数 $t_1<t_2<\dotsm<t_n<t_{n+1}$ 和 $S$ 内任意 $n+1$ 个状态 $j_1,j_2,\dotsm,j_n,j_{n+1}$ , 条件概率

$$
\begin{aligned}
    &P(X(t_{n+1})=j_{n+1}\vert X(t_1)=j_1,X(t_2)=j_2,\dotsm,X(t_n)=j_n)\\
    =&P(X(t_{n+1})=j_{n+1}\vert X(t_n)=j_n)
\end{aligned}
$$

恒成立, 则称此过程为**马尔可夫链**. 上式反映出来的性质称为**马尔可夫性**, 或无后效性.

### 离散参数马尔可夫链

...