---	
layout:     post	
title:      『Probability Statistics』 Probability Theory	
subtitle:   『概率统计』 概率论    
date:       2021-01-03	   
author:     Coekjan 
header-img: img/post-bg-PS.jpg	
catalog:    true	
katex:    true    
tags:	
    - Probability Statistics  
---

## 随机事件的概率

### 概率的公理化定义

随机试验 $E$ , 样本空间 $S$ , 设 $F=\{A\vert A\subset S \}$ , 并满足以下条件:
1. $\varnothing\notin F, S\in F$ ;
2. 若 $A\in F$ , 则 $\overline{A}\in F$ ;
3. 对任意有限个或可列个 $A_i\in F$ , 都有 $\displaystyle\sum_iA_i\in F$ .

就是说, $F$ 是一些随机事件组成的集合(且具有一定的构造关系), 称 $F$ 为事件域.

设 $P=P(A)$ 是定义在 $F$ 上的一个实值函数, $A\in F$ , 并且 $P=P(A)$ 满足下列三个条件:
1. 对于每一个 $A\in F$ , 都有 $0\le P(A)\le 1$ ;
2. $P(S)=1$ ;
3. 对任意可列个互不相容的事件 $A_1,A_2,\dotsm,A_n,\dotsm$ , 有:
   
   $$
   P\left(\sum_{i=1}^{+\infty}A_i\right)=\sum_{i=1}^{+\infty}P(A_i)
   $$

则称 $P$ 为 $F$ 上的概率测度函数, 称 $P(A)$ 为事件 $A$ 的概率.

> 称事件 $A$ 与 $B$ 互不相容(互斥), 是指 $A\cap B=\varnothing$ .

可以得到概率的一些性质:
* 不可能事件的概率为 $0$ , 即 $P(\varnothing)=0$ ;
* 概率具有有限可加性, 即若 $A_1, A_2, \dotsm, A_n$ 互不相容, 则:
  
  $$
  P\left(\sum_{i=1}^{n}A_i\right)=\sum_{i=1}^{n}P(A_i)
  $$

* 对于任意事件 $A$ , 有:
  
  $$
  P(\overline{A})=1-P(A)\qquad P(A)=1-P(\overline{A})
  $$

* 若 $B\subset A$ , 则 $P(A-B)=P(A)-P(B)$ , 且 $P(B)<P(A)$ ;
* 对于任意 $n$ 个事件 $A_1, A_2, \dotsm, A_n$ , 有:
  
  $$
  \begin{aligned}
      P\left(\sum_{i=1}^{n}A_i\right)=&\sum_{i=1}^{n}P(A_i)-\sum_{i<j}P(A_iA_j)+\dotsm\\
      +&(-1)^{k+1}\sum_{i_1<i_2<\dotsm<i_k}P(A_{i_1}A_{i_2}\dotsm A_{i_k})+\dotsm\\
      +&(-1)^{n+1}P(A_1A_2\dotsm A_n)
  \end{aligned}
  $$

  当 $n=2$ 时, 有 $P(A_1+A_2)=P(A_1)+P(A_2)-P(A_1A_2)$ .

### 条件概率与乘法公式

#### 条件概率

设 $A,B$ 为试验 $E$ 的两个事件, 且 $P(B)>0$ , 则称:

$$
P(A\vert B)=\frac{P(AB)}{P(B)}
$$

为在事件 $B$ 发生的条件下事件 $A$ 发生的条件概率.

可以得到条件概率的一些性质(以下假设 $P(B)>0$ ):
* 对于任意事件 $A$ , 有 $0\le P(A\vert B)\le1$ .
* 若事件 $A_1,A_2,\dotsm,A_i,\dotsm$ 互不相容, 则有:
  
  $$
  \begin{aligned}
      P\left(\sum_{i=1}^nA_i\vert B\right)&=\sum_{i=1}^nP(A_i\vert B)\\
      P\left(\sum_{i=1}^{+\infty}A_i\vert B\right)&=\sum_{i=1}^{+\infty}P(A_i\vert B)
  \end{aligned}
  $$

* 对于任意事件 $A$ , 有 $P(\overline{A}\vert B)=1-P(A\vert B)$

#### 乘法公式

乘法公式是指:

$$
\left.\begin{aligned}
    P(A_1A_2\dotsm A_n)&=P(A_1)P(A_2\vert A_1)P(A_3\vert A_1A_2)\dotsm P(A_n\vert A_1A_2\dotsm A_{n-1})\\
    P(A_1A_2\dotsm A_n\vert B)&=P(A_1\vert B)P(A_2\vert A_1B)P(A_3\vert A_1A_2B)\dotsm P(A_n\vert A_1A_2\dotsm A_{n-1}B)
\end{aligned}\right\}
$$

还有其他形式的乘法公式, 此处略去.

### 全概率公式与贝叶斯公式

#### 全概率公式

##### 有穷个事件的情况

设事件 $B_1,B_2,\dotsm,B_n$ 满足:
1. $\displaystyle\sum_{i=1}^nB_i=S$ ;
2. $B_1, B_2, \dotsm, B_n$ 互不相容;
3. $\forall i\in\{1,2,\dotsm,n\},P(B_i)>0$ .

则对于任意事件 $A$ , 恒有:

$$
P(A)=\sum_{i=1}^nP(B_i)P(A\vert B_i)
$$

事实上, 上述定理的条件1可以变为 $\displaystyle\sum_{i=1}^nB_i\supset A$

##### 无穷个事件的情况

设事件 $B_1,B_2,\dotsm,B_n,\dotsm$ 满足:
1. $\displaystyle\sum_{i=1}^{+\infty}B_i=S$ ;
2. $B_1,B_2,\dotsm,B_n,\dotsm$ 互不相容;
3. $\forall i\in\{1,2,\dotsm,n,\dotsm\},P(B_i)>0$ .

则对于任意事件 $A$ , 恒有:

$$
P(A)=\sum_{i=1}^{+\infty}P(B_i)P(A\vert B_i)
$$

#### 贝叶斯公式

设事件 $B_1,B_2,\dotsm,B_n$ 满足:
1. $\displaystyle\sum_{i=1}^nB_i=S$ ;
2. $B_1, B_2, \dotsm, B_n$ 互不相容;
3. $\forall i\in\{1,2,\dotsm,n\},P(B_i)>0$ .

则对于任意事件 $A$ , 恒有:

$$
P(B_i\vert A)=\frac{P(AB_i)}{A}=\frac{P(B_i)P(A\vert B_i)}{\displaystyle\sum_{j=1}^nP(B_j)P(A\vert B_j)}\quad i=1,2,\dotsm,n
$$

> 贝叶斯公式提供了从结果推测原因的方法.

### 独立事件

若事件 $B$ 的发生不影响事件 $A$ 的发生, 即:

$$
P(A\vert B)=P(A)\Leftrightarrow P(AB)=P(A)P(B)
$$

则称这两个事件相互独立.

若事件 $A$ 与 $B$ 独立, 则:
* $\overline{A}$ 与 $B$ 独立;
* $A$ 与 $\overline{B}$ 独立;
* $\overline{A}$ 与 $\overline{B}$ 独立.

#### 有限多个事件与无穷多个事件的独立性

1. 若事件 $A_1, A_2,\dotsm, A_n$ 满足:
   
   $$
   P(A_iA_j)=P(A_i)P(A_j)\quad \forall i,j\in\{1,2,\dotsm,n\}
   $$
   
   则称这 $n$ 个事件两两独立.
2. 若事件 $A_1, A_2,\dotsm, A_n$ 对任意整数 $k(2\le k\le n)$ 和 $1\le i_1<i_2<\dotsm<i_k\le n$ 恒有:
   
   $$
   P(A_{i_1}A_{i_2}\dotsm A_{i_k})=\prod_{m=1}^kP(A_{i_m})
   $$

   则称这 $n$ 个事件相互独立.
3. 对于可列无穷多个事件 $A_1,A_2,\dotsm,A_n,\dotsm$ , 若其中任意有限多个事件都相互独立, 则称这无穷多个事件相互独立.

#### 独立事件概率的一些结论

设事件 $A_1,A_2,\dotsm, A_n$ 相互独立, 则以下结论成立:
1. $P(A_1A_2\dotsm A_n)=P(A_1)P(A_2)\dotsm P(A_n)$ ;
2. $P(A_1+A_2+\dotsm+A_n)=1-P(\overline{A_1})P(\overline{A_2})\dotsm P(\overline{A_n})$ ;
3. $P(\overline{A_1}+\overline{A_2}+\dotsm+\overline{A_n})=1-P(A_1)P(A_2)\dotsm P(A_n)$

## 随机变量及其分布

### 随机变量的分布函数

设 $X$ 为随机变量, 对于任意实数 $x$ , 令:

$$
F(x)=P(X\le x),\quad-\infty<x<+\infty
$$

则称 $F(x)$ 为随机变量 $X$ 的概率分布函数, 简称分布函数, 记为 $X\sim F(x)$ .

分布函数具有一些基本性质:
1. 取值范围 $0\le F(x)\le 1$ ;
2. 单调不减, 即对于 $x_1<x_2$ , 有 $F(x_1)\le F(x_2)$ ;
3. $F(+\infty)=1,F(-\infty)=0$ ;
4. 右连续, 即对于任意实数 $x_0$ , 有 $F(x_0^+)=F(x_0)$ ;
5. 对于任意实数 $a$ , $b$ , 且 $a<b$ , 有:
   
   $$
   \left\{\begin{aligned}
     P(X>a)&=1-F(a)\\
     P(X=b)&=F(b)-F(b^-)\\
     P(a<X\le b)&=F(b)-F(a)\\
     P(a<X<b)&=F(b^-)-F(a)\\
     P(a\le X\le b)&=F(b)-F(a^-)\\
     P(a\le X <b)&=F(b^-)-F(a^-)
   \end{aligned}\right.
   $$

### 离散型随机变量及其概率分布

若随机变量 $X$ 只可能取有限个或可数个实数值: $x_1,x_2,\dotsm,x_k,\dotsm(\forall i\neq j, x_i\neq x_j)$ , 则称 $X$ 为离散型随机变量. $X$ 取各个可能值的概率:

$$
p_k=P(X=x_k)\quad k=1,2,\dotsm
$$

称为离散型随机变量 $X$ 的概率分布(或分布律, 分布列). 离散型随机变量的分布律具有一些基本性质:
1. $p_k\ge 0,k=1,2,\dotsm$ ;
2. $\displaystyle\sum_kp_k=1$ .

#### 常用离散型随机变量的分布律

##### 两点分布

若随机变量 $X$ 的分布律为 $P(X=1)=p,P(X=0)=q,0<p<1,p+q=1$ , 则称 $X$ 服从参数为 $p$ 的两点分布.

博彩中的输和赢, 抽签中的中和不中, 设备质量的好和坏, 舆论调查中的赞成和反对, 都可以看作服从两点分布.

##### 泊松分布

若随机变量 $X$ 的分布律为:

$$
P(X=k)=e^{-\lambda}\frac{\lambda^k}{k!},\quad k=0,1,2,\dotsm
$$

其中 $\lambda>0$ , 则称 $X$ 服从参数为 $\lambda$ 的泊松分布. 记作 $X\sim\Pi(\lambda)$ .

时间段 $[0,t]$ 中, 某电话交换台街道的呼叫次数, 到达某机场的飞机数, 纺织厂生产的布匹的疵点个数, 一批牧草种子中杂草的个数, 晴朗的夜晚观察某片天空出现的流行个数, 都可以看作服从泊松分布.

##### 超几何分布

设一批产品中有 $M$ 件正品, $N$ 件次品. 从中任意抽取 $n$ 件, 则渠道次品的个数 $X$ 是一个离散型随机变量, 其分布律为:

$$
P(X=k)=\frac{C_N^kC_M^{n-k}}{C_{M+N}^n}\quad k=0,1,2,\dotsm,l\quad(l=\min\{n,N\})
$$

##### 二项分布

设试验 $E$ 只有两个可能的结果 $A$ 和 $\overline{A}$ .出现 $A$ 的概率记为 $P(A)=p(0<p<1)$ , 出现 $\overline{A}$ 的概率记为 $P(\overline{A})=q=1-p$ . 将试验 $E$ 独立地重复 $n$ 次, 称这一串独立重复实验为 $n$ 重伯努利试验. $n$ 重伯努利实验中事件 $A$ 发生的次数为 $X(X=0,1,2,\dotsm,n)$ , $X$ 是一个离散型随机变量, 其分布律为:

$$
P(X=k)=C_n^kp^kq^{n-k}\quad k=0,1,2,\dotsm,n
$$

若随机变量 $X$ 服从上述分布律, 称 $X$ 服从参数为 $n,p$ 的二项分布, 记作 $X\sim B(n,p)$ .

###### 近似计算

当二项分布中 $n$ 很大和 $p$ 很小时, 概率分布 $P(X=k)=C_n^kp^kq^{n-k}$ 十分接近泊松分布的概率分布.

$$
C_n^kp^kq^{n-k}\rightarrow \frac{e^{-\lambda}\lambda^k}{k!}\quad \lambda=np(\rightarrow+\infty)
$$

当 $n\ge10,p\le0.1$ 时, 有近似公式:

$$
C_n^kp^kq^{n-k}\approx \frac{e^{-\lambda}\lambda^k}{k!}
$$

由于:

$$
\sum_{k=i}^nC_n^kp^kq^{n-k}\approx \sum_{k=i}^n\frac{e^{-\lambda}\lambda^k}{k!}=1-\sum_{k=n+1}^{+\infty}\frac{e^{-\lambda}\lambda^k}{k!}
$$

因此通过查泊松分布表, 可以得到上式的结果.

### 连续型随机变量及其概率密度函数

设随机变量 $X$ 的分布函数为 $F(x)$ , 如果存在一个定义在 $(-\infty,+\infty)$ 的非负可积函数 $f(x)$ , 使得对于任何实数 $x$ , 恒有:

$$
F(x)=\int_{-\infty}^xf(t)\operatorname{d}t
$$

则称 $X$ 为连续型随机变量, 称 $f(x)$ 为随机变量 $X$ 的概率密度函数.

概率密度函数有以下性质:
1. $\forall x\in(-\infty,+\infty),f(x)\ge 0$ ;
2. $\displaystyle\int_{-\infty}^{+\infty}f(x)\operatorname{d}x=F(+\infty)=1$ .

有以下结论:
* $F(x)$ 是连续函数;
* $\forall x\in(-\infty,+\infty),P(X=x)=0$ ;
* $\forall a<b,P(a<X\le b)=F(b)-F(a)=\displaystyle\int_a^bf(x)\operatorname{d}x$ ;
* 设区间 $I\subseteq R$ , 则 $P(X\in I)=\displaystyle\int_If(x)\operatorname{d}x$ ;
* 若 $f$ 在 $x_0$ 连续, 则 $F$ 在 $x_0$ 可导, 且 $F'(x_0)=f(x_0)$ .

#### 常用连续型随机变量分布

##### 均匀分布

设有限区间 $[a,b]$ . 若 $\zeta$ 是连续型随机变量, 并且有概率密度:

$$
f(x)=\left\{\begin{aligned}
  \frac{1}{b-a},&&&a\le x\le b\\
  0,&&&\text{Others}
\end{aligned}\right.
$$

则称 $\zeta$ 为区间 $[a,b]$ 的均匀分布, 记作 $\zeta\sim U[a,b]$ , 其分布函数为:

$$
F(x)=\left\{\begin{aligned}
  0,&&&x<a\\
  \frac{x-a}{b-a},&&&a\le x<b\\
  1,&&&x\ge b
\end{aligned}\right.
$$

##### 指数分布

若连续型随机变量 $\zeta$ 的概率密度函数为:

$$
f(x)=\left\{\begin{aligned}
  \lambda e^{-\lambda x},&&&x\ge0\\
  0,&&&x<0
\end{aligned}\right.\qquad(\lambda>0)
$$

则称 $\zeta$ 服从参数为 $\lambda$ 的指数分布, 记作 $\zeta\sim E(\lambda)$ , 其分布函数为:

$$
F(x)=\left\{\begin{aligned}
  1-e^{-\lambda x},&&&x\ge0\\
  0,&&&x<0
\end{aligned}\right.\qquad(\lambda>0)
$$

指数分布"无记忆性":

$$
X\sim E(\lambda)\Rightarrow P(X>s+t\vert X>s)=P(X>t)
$$

##### 正态分布

若连续型随机变量 $\zeta$ 的概率密度函数为:

$$
f(x)=\frac{1}{\sigma\sqrt{2\pi}}\exp\left[-\frac{(x-\mu)^2}{2\sigma^2}\right]\quad \sigma>0,-\infty<\mu<+\infty
$$

则称 $\zeta$ 服从参数为 $\mu, \sigma$ 的正态分布, 记作 $\zeta\sim N(\mu,\sigma^2)$ .

正态密度曲线有以下性质:
1. 关于直线 $x=\mu$ 对称, 且在 $x=\mu$ 处取得最大值;
2. $\displaystyle\lim_{x\rightarrow\infty}f(x)=0$ ;
3. 曲线在 $x=\mu\pm\sigma$ 处有拐点.

###### 标准正态分布

参数为 $\mu=0,\sigma=1$ 的正态分布称为**标准正态分布**. 其概率密度函数与分布函数用 $\varphi(x)$ 和 $\varPhi(x)$ 表示.

分布函数 $\varPhi(x)$ 有性质:
1. $\varPhi(0)=\displaystyle\frac{1}{2}$ ;
2. $\varPhi(x)+\varPhi(-x)=1$

若 $\xi\sim N(\mu,\sigma)$ , 则 $\zeta=\displaystyle\frac{\xi-\mu}{\sigma}\sim N(0,1)$ , 因此若 $\xi$ 的分布函数为 $F(x)$ , 则:

$$
F(x)=\varPhi\left(\frac{x-\mu}{\sigma}\right)
$$

引入**分位点**: 任意给定 $0<\alpha<1$ , 存在唯一 $z_\alpha$ , 使得 $\varPhi(z_\alpha)=\alpha$ . 称 $z_\alpha$ 为标准正态分布的 $\alpha$ 分位点.

分位点有以下性质:
1. $z_\alpha=-z_{1-\alpha}$ ;
2. $P(X>z_{1-\alpha}) = \alpha$ ;
3. $P(\vert X\vert>z_{1-\frac{\alpha}{2}})=\alpha$ 或 $P(\vert X\vert \le z_{1-\frac{\alpha}{2}})=1-\alpha$ .

## 二维随机变量

### 随机向量与联合分布

设 $(X,Y)$ 是二维随机变量, 对任意实数 $x,y$ , 二元函数

$$
F(x,y)=P(X\le x,Y\le y)
$$

称为二维随机变量 $(X,Y)$ 的联合分布函数. 这个函数有以下性质:
1. $0\le F(x,y)\le 1$ ;
2. $F$ 对 $x$ 或 $y$ 单调不减;
3. 一些极限结果:
   
   $$
   \left\{\begin{aligned}
     F(x,-\infty)&=0\\
     F(-\infty,y)&=0\\
     F(-\infty,-\infty)&=0\\
     F(+\infty,+\infty)&=1
   \end{aligned}\right.
   $$
   
4. $F(x,y)$ 对 $x$ 或 $y$ 右连续, 即:
   
   $$
   \begin{aligned}
     F(x^+,y)=F(x,y)\\
     F(x,y^+)=F(x,y)
   \end{aligned}
   $$
   
5. 对任意实数 $x_1<x_2,y_1<y_2$ 有:
   
   $$
   P(x_1<X\le x_2,y_1<Y\le y_2)=F(x_2,y_2)+F(x_1,y_1)-F(x_1,y_2)-F(x_2,y_1)
   $$


#### 二维离散型随机变量

若二维随机变量 $(X,Y)$ 的所有取值为有限对或可列对 $(x_i,y_j),i,j=1,2,\dotsm$ , 则称 $(X,Y)$ 为离散型随机变量. 记 $P(X=x_i,Y=y_j)=p_{ij},i,j=1,2,\dotsm$ , 称它为二维离散型随机变量 $(X,Y)$ 的联合分布律.

#### 二维连续型随机变量

设二维随机变量 $(X,Y)$ 的分布函数为 $F(x,y)$ , 若有非负可积函数 $f(x,y)$ 使得对于任意实数 $x,y$ 恒有:

$$
F(x,y)=\int_{-\infty}^y\int_{-\infty}^xf(u,v)\operatorname{d}u\operatorname{d}v
$$

则称 $(X,Y)$ 是二维连续型随机变量, 称 $f(x,y)$ 是连续型随机变量 $(X,Y)$ 的联合概率密度.

概率分布函数与密度函数有如下性质:
1. $F(x,y)$ 是连续函数;
2. $P(a<X\le b,c<Y\le d)=\displaystyle\int_a^b\int_c^df(x,y)\operatorname{d}y\operatorname{d}x$ ;
3. 设 $D$ 是平面上的区域, 则:
   
   $$
   P((X,Y)\in D)=\iint_Df(x,y)\operatorname{d}x\operatorname{d}y
   $$

##### 常用的二维连续型随机变量

###### 均匀分布

若随机变量 $(X,Y)$ 的概率密度为

$$
f(x,y)=\left\{\begin{aligned}
  \frac{1}{A},&&&(x,y)\in D\\
  0,&&&(x,y)\notin D
\end{aligned}\right.
$$

其中 $A=\displaystyle\iint_D\operatorname{d}\sigma$ 是有界区域 $D$ 的面积. 则称 $(X,Y)$ 在区域 $D$ 上服从均匀分布, 记作 $(X,Y)\sim U(D)$ .

###### 二维正态分布

若随机变量 $(X,Y)$ 的概率密度函数为

$$
f(x,y)=\frac{1}{2\pi\sigma_1\sigma_2\sqrt{1-\rho^2}}\exp\left\{-\frac{1}{2(1-\rho^2)}\left[\frac{(x-\mu_1)^2}{\sigma_1}-2\rho\frac{(x-\mu_1)(y-\mu_2)}{\sigma_1\sigma_2}+\frac{(y-\mu_2)^2}{\sigma_2}\right]\right\}
$$

其中:

$$
-\infty<\mu_{1,2}<+\infty,\quad\sigma_{1,2}>0,\quad\vert\rho\vert<1
$$

则称随机变量 $(X,Y)$ 服从参数为 $\mu_1,\mu_2,\sigma_1,\sigma_2,\rho$ 的二维正态分布, 记作: $(X,Y)\sim N(\mu_1,\sigma^2;\mu_2,\sigma_2;\rho)$ .

### 边缘分布函数

由 $(X,Y)$ 的分布函数 $F(x,y)$ 可以确定 $X$ 和 $Y$ 的边缘分布函数:

$$
\begin{aligned}
  F_X(x)&=F(x,+\infty)\\
  F_Y(y)&=F(+\infty,y)
\end{aligned}
$$

#### 二维离散型随机变量的边缘分布律与条件分布律

##### 边缘分布律

设二维离散型随机变量 $(X,Y)$ 的分布律为

$$
P(X=x_i,Y=y_j)=p_{ij},i,j=1,2,\dotsm
$$

则 $(X,Y)$ 的边缘分布律:

$$
\left\{\begin{aligned}
  p_{i\cdot}&=P(X=x_i)&=\sum_jp_{ij}\\
  p_{\cdot j}&=P(Y=y_j)&=\sum_ip_{ij}
\end{aligned}\right.
$$

> 对分布表的列和行分别求和, 即可得到相应的边缘分布律.

##### 条件分布律

设二维离散型随机变量 $(X,Y)$ 的分布律为

$$
P(X=x_i,Y=y_j)=p_{ij},i,j=1,2,\dotsm
$$

当 $P(X=x_i)>0$ 时称

$$
P(Y=y_j\vert X=x_i)=\frac{P(X=x_i,Y=y_j)}{P(X=x_i)},j=1,2,\dotsm
$$

为在 $X=x_i$ 条件下的条件分布律.

当 $P(Y=y_j)>0$ 时称

$$
P(X=x_i\vert Y=y_j)=\frac{P(X=x_i,Y=y_j)}{P(Y=y_j)},i=1,2,\dotsm
$$

为在 $Y=y_j$ 条件下的条件分布律.

#### 二维连续型随机变量的边缘概率密度与条件概率密度

##### 边缘概率密度

设二维连续型随机变量 $(X,Y)$ 的概率密度为 $f(x,y)$ , 分布函数为 $F(x,y)$ , 则 $(X,Y)$ 的边缘概率密度:

$$
\left\{\begin{aligned}
  f_X(x)=\int_{-\infty}^{+\infty}f(x,y)\operatorname{d}y\\
  f_Y(y)=\int_{-\infty}^{+\infty}f(x,y)\operatorname{d}x
\end{aligned}\right.
$$

##### 条件概率密度

对于二维连续型随机变量 $(X,Y)$ , 若存在极限:

$$
\lim_{\varepsilon\rightarrow0^+}P(X\le x\vert y-\varepsilon<Y\le y+\varepsilon)
$$

则称此极限为条件 $Y=y$ 下 $X$ 的条件分布函数, 记为 $F_{X\vert Y}(x\vert y)$ 或 $P(X\le x\vert Y=y)$ .

可计算:

$$
\begin{aligned}
  F_{X\vert Y}(x\vert y)&=\frac{\displaystyle\frac{\partial}{\partial y}F(x,y)}{\displaystyle\frac{\operatorname{d}}{\operatorname{d}y}F_Y(y)}\\
  F_{Y\vert X}(y\vert x)&=\frac{\displaystyle\frac{\partial}{\partial x}F(x,y)}{\displaystyle\frac{\operatorname{d}}{\operatorname{d}x}F_X(x)}
\end{aligned}
$$

对应的条件概率密度:

$$
\begin{aligned}
  f_{X\vert Y}(x\vert y)&=\frac{f(x,y)}{f_Y(y)}\\
  f_{Y\vert X}(y\vert x)&=\frac{f(x,y)}{f_X(x)}
\end{aligned}
$$

### 相互独立的随机变量

称随机变量 $X$ 与 $Y$ 相互独立, 是指对任意实数 $x$ 和 $y$ , 有: $P(X\le x,Y\le y)=P(X\le x)P(Y\le y)$ .

有判定定理:

$$
P(X\le x,Y\le y)=P(X\le x)P(Y\le y)\Leftrightarrow F(x,y)=F_X(x)F_Y(y)
$$

#### 离散型随机变量相互独立

设二维离散型随机变量 $(X,Y)$ 的分布律:

$$
P(X=x_i,Y=y_j)=p_{ij}\quad i,j=1,2,\dotsm
$$

则称 $X$ 和 $Y$ 相互独立的充要条件是

$$
P(X=x_i,Y=y_j)=P(X=x_i)P(Y=y_j)\Leftrightarrow p_{ij}=p_{\cdot j}\cdot p_{i\cdot}
$$

#### 连续型随机变量相互独立

设二维连续型随机变量 $(X,Y)$ 的概率密度为 $f(x,y)$ . 则 $X$ 和 $Y$ 相互独立的充要条件为

$$
f(x,y)=f_X(x)f_Y(y)
$$

#### 有限多个或可列个随机变量的相互独立

设 $X_1,X_2,\dotsm,X_n$ 为 $n$ 个随机变量, 若对于任意实数 $x_1,x_2,\dotsm,x_n$ 都有

$$
P(X_1\le x_1,X_2\le x_2,\dotsm,X_n\le x_n)=P(X_1\le x_1)P(X_2\le x_2)\dotsm P(X_n\le x_n)
$$

有充要判定条件:

$$
\begin{aligned}
  P(X_1\le x_1,X_2\le x_2,\dotsm,X_n\le x_n)&=P(X_1\le x_1)P(X_2\le x_2)\dotsm P(X_n\le x_n)\\
  &\Updownarrow\\
  F(x_1,x_2,\dotsm,x_n)&=F_{X_1}(x_1)F_{X_2}(x_2)\dotsm F_{X_3}(x_3)
\end{aligned}
$$

针对连续型随机变量, 有充要条件:

$$
\begin{aligned}
  P(X_1\le x_1,X_2\le x_2,\dotsm,X_n\le x_n)&=P(X_1\le x_1)P(X_2\le x_2)\dotsm P(X_n\le x_n)\\
  &\Updownarrow\\
  F(x_1,x_2,\dotsm,x_n)&=F_{X_1}(x_1)F_{X_2}(x_2)\dotsm F_{X_3}(x_3)\\
  &\Updownarrow\\
  f(x_1,x_2,\dotsm,x_n)&=f_{X_1}(x_1)f_{X_2}(x_2)\dotsm f_{X_3}(x_3)
\end{aligned}
$$

> 设 $X_1,X_2,\dotsm,X_n,\dotsm$ 为可列无穷多个随机变量, 若对任意整数 $k(k\ge2)$ 及任意互不相同的正整数 $i_1<i_2<\dotsm<i_k$ , $X_{i_1},X_{i_2},\dotsm,X_{i_k}$ 都相互独立, 则称这无穷多个随机变量相互独立.

