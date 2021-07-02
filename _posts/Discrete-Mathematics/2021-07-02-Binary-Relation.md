---	
layout:     post	
title:      『Discrete Mathematics』 Binary Relation	
subtitle:   『离散数学』 二元关系    
date:       2021-07-02	   
author:     Coekjan 
header-img: img/post-bg-DM.jpg	
catalog:    true
katex:  true    
tags:	
    - Discrete Mathematics  
---

## 关系

### 基本概念

**定义**: 设 $n\in\bm{I_+}$ 且 $A_1,A_2,\dotsm,A_n$ 为 $n$ 个任意集合, $R\sube\displaystyle\prod_{i=1}^nA_i$ .
1. 称 $R$ 为 $A_1,A_2,\dotsm,A_n$ 间的 $n$ 元关系.
2. 若 $n=2$ , 则称 $R$ 为 $A_1$ 到 $A_2$ 的二元关系.
3. 若 $R=\varnothing$ , 则称 $R$ 为空关系; $R=\displaystyle\prod_{i=1}^nA_i$ , 则称 $R$ 为全关系.
4. 若 $A_1=A_2=\dotsm=A_n=A$ , 则称 $R$ 为 $A$ 上的 $n$ 元关系.

**定义**: 设 $R_1$ 为 $A_1,A_2,\dotsm,A_n$ 间的 $n$ 元关系, $R_2$ 为 $B_1,B_2,\dotsm,B_m$ 间的 $m$ 元关系. 若:
1. $n=m$
2. $\forall i(1\le i\le n\rightarrow A_i=B_i)$
3. $R_1=R_2\quad(\text{As Set})$

则称 $n$ 元关系 $R_1$ 与 $m$ 元关系 $R_2$ 相等, 仍记为 $R_1=R_2$ .

**定义**: 设 $R$ 是从集合 $A$ 到集合 $B$ 的二元关系, 则称 $R_1\cap R_2$ , $R_1\cup R_2$ , $R_1-R_2$ , $R_1\oplus R_2$ 仍为 $A$ 到 $B$ 的二元关系, 分别称为二元关系 $R_1$ 与 $R_2$ 的交, 并, 差和对称差.

**定义**: 设 $R$ 是从集合 $A$ 到集合 $B$ 的二元关系, 令:

$$
\begin{aligned}
    \operatorname{dom}R&=\lbrace x\mid x\in A\land\exist y(y\in B\land<x,y>\in R)\rbrace\\
    \operatorname{ran}R&=\lbrace y\mid y\in B\land\exist x(x\in A\land<x,y>\in R)\rbrace
\end{aligned}
$$

则称 $\operatorname{dom}R$ 为 $R$ 的**定义域**, $\operatorname{ran}R$ 为 $R$ 的**值域**.

**定义**: 若 $<x,y>\in R$ 则称 $x$ 与 $y$ 有关系 $R$ , 记为 $xRy$ ; 若 $<x,y>\notin R$ 则称 $x$ 与 $y$ 无关系 $R$ , 记为 $x\bcancel{R}y$ .

**定义**: 设 $R$ 为集合 $A$ 上的二元关系.
1. 若 $\forall a(a\in A\rightarrow<a,a>\in R)$ , 则称 $R$ 是**自反**的.
2. 若 $\forall a(a\in A\rightarrow<a,a>\notin R)$ , 则称 $R$ 是**反自反**的.
3. 若 $\forall a\forall b(a\in A\land b\in A\rightarrow(<a,b>\in R\rightarrow<b,a>\in R))$ , 则称 $R$ 是**对称**的.
4. 若 $\forall a\forall b(a\in A\land b\in A\rightarrow(<a,b>\in R\land<b,a>\in R)\rightarrow a=b)$ , 则称 $R$ 是**反对称**的.
5. 若 $\forall a\forall b\forall c(a\in A\land b\in A\land c\in A\rightarrow(<a,b>\in R\land<b,c>\in R\rightarrow<a,c>\in R))$ , 则称 $R$ 是**传递**的.

> 注意此处的定义可有等价表述:
> 
> $$
> P\rightarrow(Q\rightarrow R)\iff P\land Q\rightarrow R
> $$

**定义**: 设 $R$ 为集合 $A$ 上的二元关系且 $S\sube A$ , 则称 $S$ 上的二元关系 $R\cap(S\times S)$ 为 $R$ 在 $S$ 上的压缩, 记为 $R\mid_S$ , 并称 $R$ 为 $R\mid_S$ 在 $A$ 上的延拓.

### 基本定理

**定理**: 设 $R$ 为集合 $A$ 上的二元关系且 $S\sube A$ .
1. 若 $R$ 是自反的, 则 $R\mid_S$ 也是自反的.
2. 若 $R$ 是反自反的, 则 $R\mid_S$ 也是反自反的.
3. 若 $R$ 是对称的, 则 $R\mid_S$ 也是对称的.
4. 若 $R$ 是反对称的, 则 $R\mid_S$ 也是反对称的.
5. 若 $R$ 是传递的, 则 $R\mid_S$ 也是传递的.

## 关系矩阵与关系图

> 此处仅讨论有限集到有限集的二元关系.

### 基本概念

**定义**: 设 $A$ 和 $B$ 为任意的非空有限集, $R$ 为任意一个从 $A$ 到 $B$ 的二元关系. 以 $A\cup B$ 中的每一个元素为一个结点, 对每一个 $<x,y>\in R$ 皆画一条 $x$ 到 $y$ 的有向边, 就得到一个有向图 $G_R$ , 称之为 $R$ 的**关系图**.

**定义**: 设 $n,m\in\bm{I_+}$ , $A=\lbrace x_1,x_2,\dotsm,x_n\rbrace,B=\lbrace y_1,y_2,\dotsm,y_m\rbrace$ . 对任意从 $A$ 到 $B$ 的二元关系 $R$ , 令:

$$
\begin{aligned}
    M_R&=\begin{bmatrix}
        a_{11} & a_{12} & \dotsm & a_{1m}\\
        a_{21} & a_{22} & \dotsm & a_{2m}\\
        \vdots & \vdots & \ddots & \vdots\\
        a_{n1} & a_{n2} & \dotsm & a_{nm}
    \end{bmatrix}\\
    a_{ij}&=\begin{cases}
        1 & x_iRy_j\\
        0 & x_i\bcancel{R}y_j
    \end{cases}\quad(1\le i\le n\land 1\le j\le m)
\end{aligned}
$$

称 $M_R$ 为 $R$ 的**关系矩阵**.

### 基本定理

**定理**: 若 $A$ 和 $B$ 为非空有限集, $R_1$ 和 $R_2$ 为从 $A$ 到 $B$ 的二元关系, 则下列条件等价:
1. $R_1=R_2$
2. $M_{R_1}=M_{R_2}$
3. $G_{R_1}=G_{R_2}$

**定理**: 设 $n\in\bm{I_+}$ , $R$ 为 $A=\lbrace x_1,x_2\dotsm,x_n\rbrace$ 上的二元关系, $a_{ij}(1\le i,j\le n)$ 为 $R$ 的关系矩阵 $M_R$ 中第 $i$ 行第 $j$ 列的元素. 
1. 以下条件等价:
   * $R$ 是自反的.
   * $M_R$ 对角线元素全为 $1$ .
   * $G_R$ 中每个结点 $x_i\in A$ 处都有自圈.
2. 以下条件等价:
   * $R$ 是反自反的.
   * $M_R$ 对角线元素全为 $0$ .
   * $G_R$ 中每个结点 $x_i\in A$ 处都无自圈.
3. 以下条件等价:
   * $R$ 是对称的.
   * $M_R$ 是对称矩阵.
   * $G_R$ 中任意两个结点间都不会只有单向的有向边.
4. 以下条件等价:
   * $R$ 是反对称的.
   * 在 $M_R$ 中, $\forall i\forall j(1\le i\le n\land1\le j\le n\land i\neq j\rightarrow a_{ij}\cdot a_{ji}=0)$ .
   * $G_R$ 中任意两个结点之间都无成对出现的反向有向边.
5. 以下条件等价:
   * $R$ 是传递的.
   * 在 $M_R$ 中, $\forall i\forall j(1\le i\le n\land 1\le j\le n\land\exist k(1\le k\le n\land a_{ik}\cdot a_{kj}=1)\rightarrow a_{ij}=1)$ .
   * 对 $G_R$ 中任意两个结点 $x_i$ 和 $x_j$ , 若有 $x_i$ 到 $x_j$ 的有向路径, 则必然有 $x_i$ 到 $x_j$ 的有向边.

## 逆关系

### 基本概念

**定义**: 设 $R$ 为从集合 $A$ 到集合 $B$ 的二元关系. 如果从 $A$ 到 $B$ 的二元关系 $R'$ 满足:

$$
\forall<x,y>(<x,y>\in R\leftrightarrow<x,y>\notin R')
$$

则称 $R'$ 为 $R$ 的**补关系**, 记为$\text{\textasciitilde}R$.

**定义**: 设 $R$ 为从集合 $A$ 到集合 $B$ 的二元关系. 如果从 $B$ 到 $A$ 的二元关系 $R'$ 满足:

$$
\forall<x,y>(<x,y>\in R\leftrightarrow<y,x>\in R')
$$

则称 $R'$ 为 $R$ 的**逆关系**, 记为 $R^{-1}$ .

### 基本定理

**定理**: 设 $A$ 和 $B$ 为非空有限集, $R$ 为从 $A$ 到 $B$ 的二元关系.
1. $M_{R^{-1}}=M_R^T$
2. 把 $G_R$ 的每一个有向边反向后, 就得到 $R^{-1}$ 的关系图 $G_{R^{-1}}$

**定理**: 若 $R$ 和 $R_i(i=0,1,2,\dotsm)$ 都是从集合 $A$ 到 集合 $B$ 的二元关系, $K$ 为 $\bm{N}$ 的非空子集. 则:
1. $(R^{-1})^{-1}=R$
2. $(\text{\textasciitilde}R)^{-1}=\text{\textasciitilde}R^{-1}$
3. $R_1\sube R_2\implies R_1^{-1}\sube R_2^{-1}$
4. $R_1= R_2\implies R_1^{-1}= R_2^{-1}$
5. $\displaystyle\left(\bigcup_{n\in K}R_n\right)^{-1}=\bigcup_{n\in K}R_n^{-1}$
   > * 任取 $<x,y>\in\displaystyle\left(\bigcup_{n\in K}R_n\right)^{-1}$ , 则 $<y,x>\in\displaystyle\bigcup_{n\in K}R_n$ . 因此 $\exist n_0(n_0\in K\land<y,x>\in R_{n_0})$ , 从而有 $<x,y>\in R_{n_0}$ . 因此:
   >   
   >   $$
   >   \displaystyle\left(\bigcup_{n\in K}R_n\right)^{-1}\sube\bigcup_{n\in K}R_n^{-1}
   >   $$
   > 
   > * 任取 $<x,y>\in\displaystyle\bigcup_{n\in K}R_n^{-1}$ , 则 $\exist n_0(n_0\in K\land<x,y>\in R_{n_0}^{-1})$ , 所以 $<y,x>\in R_{n_0}$ , 得到 $<y,x>\in\displaystyle\bigcup_{n\in K}R_n$ , 则有 $<x,y>\in\displaystyle\left(\bigcup_{n\in K}R_n\right)^{-1}$ . 因此:
   >   
   >   $$
   >   \bigcup_{n\in K}R_n^{-1}\sube\displaystyle\left(\bigcup_{n\in K}R_n\right)^{-1}
   >   $$

6. $\displaystyle\left(\bigcap_{n\in K}R_n\right)^{-1}=\bigcap_{n\in K}R_n^{-1}$
7. $(R_1-R_2)^{-1}=R_1^{-1}-R_2^{-1}$
8. $(R_1\oplus R_2)^{-1}=R_1^{-1}\oplus R_2^{-1}$

**定理**: 设 $R$ 为集合 $A$ 上的二元关系, 则:
1. $R$ 是自反的当且仅当 $R^{-1}$ 是自反的.
2. $R$ 是反自反的当且仅当 $R^{-1}$ 是反自反的.
3. $R$ 是对称的当且仅当 $R^{-1}$ 是对称的.
4. $R$ 是反对称的当且仅当 $R^{-1}$ 是反对称的.
5. $R$ 是传递的当且仅当 $R^{-1}$ 是传递的.

**定理**: 设 $R$ 为集合 $A$ 上的二元关系, 则 $R$ 是对称的当且仅当 $R=R^{-1}$ .

**定理**: 设 $R$ 为任意二元关系, 则:
1. $\operatorname{dom}R^{-1}=\operatorname{ran}R$
2. $\operatorname{ran}R^{-1}=\operatorname{dom}R$

## 关系的合成

### 基本概念

**定义**: 设 $R_1$ 为从集合 $A$ 到集合 $B$ 的二元关系, $R_2$ 为从集合 $B$ 到集合 $C$ 的二元关系. 称从 $A$ 到 $C$ 的二元关系:

$$
\lbrace <x,z>\mid\exist y(y\in B\land<x,y>\in R_1\land<y,z>\in R_2)\rbrace
$$

为 $R_1$ 与 $R_2$ 的合成, 记为 $R_1\circ R_2$ .

![]({{ '/img/DM-Composition-of-Relations.svg' | prepend: site.baseurl}})

> 注意, 关系的合成一般是不可交换顺序的.

### 基本定理

**定理**: 设 $A$ , $B$ , $C$ 和 $D$ 为任意的四个集合, $R_1$ 为从 $A$ 到 $B$ 的二元关系, $R_2$ 和 $R_3$ 都是从 $B$ 到 $C$ 的二元关系, $R_4$ 为从 $C$ 到 $D$ 的二元关系.
1. $R_2\sube R_3\implies R_1\circ R_2\sube R_1\circ R_3\land R_2\circ R_4\sube R_3\sube R_4$
2. $R_1\circ(R_2\cup R_3)=(R_1\circ R_2)\cup(R_1\circ R_3)$
3. $(R_2\cup R_3)\circ R_4=(R_2\circ R_4)\cup(R_3\circ R_4)$
4. $R_1\circ(R_2\cap R_3)\sube(R_1\circ R_2)\cap(R_1\circ R_3)$
5. $(R_2\cap R_3)\circ R_4\sube(R_2\circ R_4)\cap(R_3\circ R_4)$
6. $(R_1\circ R_2)^{-1}=R_2^{-1}\circ R_1^{-1}$
7. $(R_1\circ R_2)\circ R_4=R_1\circ (R_2\circ R_4)$

### 合成关系的关系矩阵

为研究合成关系的的关系矩阵, 在集合 $\lbrace0,1\rbrace$ 上定义两个运算 $\land$ 和 $\lor$ :

$$
\begin{aligned}
    0\lor0&=0\\
    0\lor1&=1\lor0=1\lor1=1\\
    0\land0&=1\land0=0\land1=0\\
    1\land1&=1
\end{aligned}
$$

定义 $0-1$ 矩阵间的运算 $\circledast$ :

$$
\begin{aligned}
    \begin{bmatrix}
        a_{11} & a_{12} & \dotsm & a_{1m}\\
        a_{21} & a_{22} & \dotsm & a_{2m}\\
        \vdots & \vdots & \ddots & \vdots\\
        a_{n1} & a_{n2} & \dotsm & a_{nm}
    \end{bmatrix}&\circledast\begin{bmatrix}
        b_{11} & b_{12} & \dotsm & b_{1p}\\
        b_{21} & b_{22} & \dotsm & b_{2p}\\
        \vdots & \vdots & \ddots & \vdots\\
        b_{m1} & b_{m2} & \dotsm & b_{mp}
    \end{bmatrix}=\begin{bmatrix}
        c_{11} & c_{12} & \dotsm & c_{1p}\\
        c_{21} & c_{22} & \dotsm & c_{2p}\\
        \vdots & \vdots & \ddots & \vdots\\
        c_{n1} & c_{n2} & \dotsm & c_{np}
    \end{bmatrix}\\
    c_{ij}&=\bigvee_{k=1}^m(a_{ik}\land b_{kj})\quad(1\le i\le n\land 1\le j\le p)\\
    \text{or}\qquad c_{ij}&=1 \iff \exist k(1\le k\le m\land a_{ik}=b_{kj}=1)
\end{aligned}
$$

**定理**: 若 $A$ , $B$ , $C$ 都是非空有限集, $R_1$ 为从 $A$ 到 $B$ 的二元关系, $R_2$ 为从 $B$ 到 $C$ 的二元关系, 则 $M_{R_1\circ R_2}=M_{R_1}\circledast M_{R_2}$ .

### 自合成

**定义**: 设 $A$ 为任意集合, $R$ 为 $A$ 上的任意二元关系. 令:
1. $R^0=\bm{I}_A$
2. $R^{n+1}=R\circ R^n,n\in\bm{N}$

其中 $\bm{I}_A=\lbrace <x,x>\mid x\in A\rbrace$ 为 $A$ 上的恒等关系. 称 $R^n(n\in\bm{N})$ 为 $R$ 的 $n$ 次幂.

**定理**: 若 $n,m\in\bm{N}$ 且 $R$ 为集合 $A$ 上的二元关系, 则:
1. $(R^n)^{-1}=(R^{-1})^n$
2. $R^n\circ R^m=R^{n+m}$
3. $(R^m)^n=R^{mn}$

> 可使用归纳法证明.

**定理**: 设有限集 $A$ 恰有 $n$ 个元素. 若 $R$ 为 $A$ 上的二元关系, 则 $\exist s\exist t(s\in\bm{N}\land t\in\bm{N}\land s<t\le 2^{n^2}\land R^s=R^t)$ .
> 首先, $A\times A$ 中共有 $n^2$ 个元素. 因此 $A\times A$ 的子集个数为 $2^{n^2}$ , 即 $A$ 上共有 $2^{n^2}$ 个不同的二元关系. 根据鸽巢原理, 在下述 $2^{n^2}+1$ 个 $A$ 上的二元关系:
> 
> $$
> R^0,R^1,R^2,\dotsm,R^{2^{n^2}}
> $$
> 
> 中, 必有两个相同的二元关系. 即 $\exist s\exist t(s\in\bm{N}\land t\in\bm{N}\land s<t\le 2^{n^2}\land R^s=R^t)$ .

## 关系的闭包

### 基本概念

**定义**: 设 $R$ 为集合 $A$ 上的二元关系. 如果 $A$ 上的二元关系 $R'$ 满足:
1. $R'$ 是自反的(或对称的, 或传递的)
2. $R\sube R'$
3. 若 $A$ 上的二元关系 $R''$ 也满足1和2, 则 $R'\sube R''$

则称 $R'$ 为 $R$ 的自反(或对称, 或传递)闭包, 记为 $r(R)$ (或 $s(R)$ , 或 $t(R)$ )

### 基本定理

> 引入记号:
> 
> $$
> \begin{aligned}
>     R^+&=\bigcup_{n=1}^\infty R^n\\
>     R^*&=\bigcup_{n=0}^\infty R^n
> \end{aligned}
> $$

**定理**: 设 $R$ 为集合 $A$ 上的二元关系.
1. $R$ 是自发的, 当且仅当 $r(R)=R$ .
2. $R$ 是对称的, 当且仅当 $s(R)=R$ .
3. $R$ 是传递的, 当且仅当 $t(R)=R$ .

**定理**: 设 $R$ 为集合 $A$ 上的二元关系.
1. $r(R)=R\cup\bm{I}_A$
2. $s(R)=R\cup R^{-1}$
3. $t(R)=R^+$
   > * 若 $<x,y>,<y,z>\in R^+$ , 则必有 $n,m\in\bm{I_+}$ 使得 $<x,y>\in R^n\land<y,z>\in R^m$ . 因此, $<x,z>\in R^{n+m}$ . 所以 $<x,z>\in R^+$ , 即 $R^+$ 是传递的. 因此, $t(R)\sube R^+$ .
   > * 可用归纳法证明 $\forall n(n\in\bm{I_+}\rightarrow R^n\sube t(R))$ :
   >   1. 根据定义, $R\sube t(R)$ 是显然的.
   >   2. 对于任意的 $k\in\bm{I_+}$ , 假定当 $n=k$ 时命题为真, 即 $R^k\sube t(R)$ . 任取 $<x,z>\in R^{k+1}$ , 因为 $R^{k+1}=R\circ R^k$ , 所以存在 $y\in A$ , 使得 $<x,y>\in R\land<y,z>\in R^k$ . 但由于 $R\sube t(R)\land R^k\sube t(R)$ , 故 $<x,y>\in t(R)\land <y,z>\in t(R)$ . 由于 $t(R)$ 是传递的, 故 $<x,z>\in t(R)$ . 这说明 $R^{k+1}\sube t(R)$ , 即当 $n=k+1$ 时命题为真.
   >   
   >   故而有 $R^+\sube t(R)$ .

**定理**: 设 $A$ 为含有 $n$ 个元素的有限集, $R$ 为 $A$ 上的二元关系, 则:

$$
t(R)=\bigcup_{i=1}^nR^i
$$

> * 若 $n=0$ , 则 $t(R)=\varnothing=\displaystyle\bigcup_{i=1}^0R^i$ .
> * 若 $n>0$ , 使用第二归纳法证明 $\forall k\left(k\in\bm{N}\rightarrow R^{n+k}\sube\displaystyle\bigcup_{i=1}^n R^i\right)$ .
>   1. 当 $k=0$ 时, $R^{n+k}=R^n\sube\displaystyle\bigcup_{i=1}^n R^i$ .
>   2. 设 $m\in\bm{I_+}$ , 假设 $k<m$ 时皆有 $R^{n+k}\sube\displaystyle\bigcup_{i=1}^nR^i$ . 任取 $<x,y>\in R^{n+m}$ , 令 $u_0=x,u_{n+m}=y$ , 则有 $u_1,u_2,\dotsm,u_{n+m-1}\in A$ 使得:
>      $$
>      <u_i,u_{i+1}>\in R\quad(i=0,1,\dotsm,n+m-1)
>      $$
>      
>      因为 $n+m>n$ , 根据鸽巢原理, 在 $u_1,u_2,\dotsm,u_{n+m}$ 这 $n+m$ 个元素中必有两个相同, 设 $u_l=u_j(1\le l<j\le n+m)$ , 因此有 $<u_0,u_1>,\dotsm,<u_{l-1},u_l>,<u_l,u_{j+1}>,\dotsm,<u_{n+m-1}, u_{n+m}>\in R$ , 即 $<x,y>\in R^{n+m-(j-l)}$ .
>      
>      根据假设, 可得 $R^{n+m-(j-l)}\sube \displaystyle\bigcup_{i=1}^nR^i$ , 得 $<x,y>\in\displaystyle\bigcup_{i=1}^nR^i$ . 这说明 $R^{n+m}\sube\displaystyle\bigcup_{i=1}^nR^i$ .
>   
>   因此, $\forall k\left(k\in\bm{N}\rightarrow R^{n+k}\sube\displaystyle\bigcup_{i=1}^n R^i\right)$ .

**定理**: 若 $R_1$ 和 $R_2$ 都是集合 $A$ 上得二元关系且 $R_1\sube R_2$ , 则:
1. $r(R_1)\sube r(R_2)$ .
2. $s(R_1)\sube s(R_2)$ .
3. $t(R_1)\sube t(R_2)$ .

**定理**: 设 $R$ 为集合 $A$ 上的二元关系.
1. 若 $R$ 是自反的, 则 $s(R), t(R)$ 都是自反的.
2. 若 $R$ 是对称的, 则 $r(R), t(R)$ 都是对称的.
3. 若 $R$ 是传递的, 则 $r(R)$ 是对称的.

**定理**: 设 $R$ 为集合 $A$ 上的二元关系.
1. $rs(R)=sr(R)$
2. $rt(R)=tr(R)$
3. $st(R)\sube ts(R)$

## 相容关系

### 相容与基本定理

#### 基本概念与基本定理

**定义**: 如果集合 $A$ 上的二元关系 $R$ 是自反的和对称的, 则称 $R$ 为 $A$ 上的相容关系. 此时, 若 $xRy$ 则称 $x$ 与 $y$ 相容; 否则称 $x$ 与 $y$ 不相容.

**定理**: 若 $R$ 为集合 $A$ 上的二元关系, 则 $R$ 为 $A$ 上的相容关系当且仅当 $r(R)=s(R)=R$ .

#### 简化关系矩阵与简化关系图

设 $R$ 为非空有限集 $A=\lbrace x_1,x_2\dotsm,x_n\rbrace$ 上的相容关系. 因为 $R$ 是自反的和对称的, 所以 $R$ 的关系矩阵 $M_R$ 和关系图 $G_R$ 可以简化.

首先考虑对 $M_R$ 的简化. 因为 $M_R$ 是对称的且对角线上全为 $1$ . 因此只需知道 $M_R$ 对角线以下的元素即可. 若 $n>1$ , 且以 $a_{ij}$ 表示 $M_R$ 的第 $i$ 行第 $j$ 列元素时, 则 $M_R$ 可以用如下三角阵代替:

$$
\begin{array}{c|c}
    x_2 & a_{21} \\
    x_3 & a_{31} & a_{32} \\
    \vdots & \vdots & \vdots & \ddots \\
    x_n & a_{n1} & a_{n2} & \dotsm & a_{n(n-1)} \\
    \hline
        & x_1 & x_2 & \dotsm & x_{n-1}
\end{array}
$$

其次考虑对 $G_R$ 的简化. 因为 $G_R$ 中每个结点处都有一个自圈, 而且任意两个结点之间都不会仅有单向边, 所以我们可以把 $G_R$ 中所有的自圈去掉, 并把每对反向的有向边都改为一条无向边. 称这样的无向图为 $R$ 的简化关系图.

### 相容类与极大相容类的求解法

**定义**: 设 $R$ 为集合 $A$ 上的相容关系.
1. 如果 $S$ 为 $A$ 的非空子集, 且 $\forall x\forall y(x\in S\land y\in S\rightarrow xRy)$ , 则称 $S$ 为 $R$ 的一个相容类.
2. 设 $S$ 为 $R$ 的相容类. 若 $\forall y(y\notin S\rightarrow\exist x(x\in S\land x\bcancel{R}y))$ , 则称 $S$ 为 $R$ 的一个极大相容类.

#### 关系图法

> * 称一个多边形是完全多边形, 是指其中任意两个结点之间都有一条无向边.
> * 称一个多边形是极大完全多边形, 是指给它添加图中任意的另一个结点, 它就不再是完全多边形.

先画出 $R$ 的简化关系图, 则其中每一个极大完全多边形的顶点集合, 就是 $R$ 的一个极大相容类.

#### 关系矩阵法

算法:
1. 列出 $R$ 的简化关系矩阵.
2. $R$ 的所有第 $n$ 级相容类为 $\lbrace x_1\rbrace,\lbrace x_2\rbrace,\dotsm,\lbrace x_n\rbrace$ .
3. 若 $n=1$ 则算法结束.
4. 若 $n>1$ 则令 $i\leftarrow n-1$ .
5. $A\leftarrow\lbrace x_j\mid a_{ji}=1\land i<j\le n\rbrace$
6. 对于每一个 $i+1$ 级相容类 $S$ , 若 $S\cap A\neq\varnothing$ , 则添加一个新的相容类 $\lbrace x_i\rbrace\cup(S\cap A)$ .
7. 对于已得到的任意两个相容类 $S_1$ 和 $S_2$ , 若 $S_1\sube S_2$ , 则删去 $S_1$ . 并称这样合并得到的所有相容类为第 $i$ 级相容类.
8. 若 $i>1$ , 则 $i\leftarrow i-1$ , 并转5.
9. 若 $i=1$ 则算法结束.

最后得到的所有相容类, 就是 $R$ 的所有极大相容类.

## 等价关系

### 等价与基本定理

**定义**: 如果集合 $A$ 上的二元关系 $R$ 是自反的, 对称的和传递的, 则称 $R$ 为 $A$ 上的等价关系. 若 $\exist x\exist y(x\in A\land y\in A\land xRy)$ 则称 $x$ 与 $y$ 等价, 记为 $x\approx_Ry$ . 在不强调 $R$ 时, 常简记为 $x\approx y$ .

**定理**: 如果 $R$ 为集合 $A$ 上的二元关系, 则 $R$ 为 $A$ 上的等价关系的充要条件为:

$$
r(R)=s(R)=t(R)=R
$$

**定理**: 如果 $R$ 为集合 $A$ 上的二元关系, 则 $tsr(R), trs(R), rts(R)$ 都是 $A$ 上的等价关系.

### 等价关系图与关系矩阵

**定理**: 若 $R$ 为非空有限集 $A$ 上的二元关系, 则 $R$ 为 $A$ 上的等价关系的充要条件为: $R$ 有简化关系图, 且每一个分支都是完全图.

**定理**: 若 $R$ 为非空有限集 $A$ 上的二元关系, 则 $R$ 为 $A$ 上的等价关系的充要条件为:
1. $M_R$ 的对角线元素全为 $1$ .
2. $M_R$ 是对称矩阵.
3. $M_R$ 可以经过有限次地把行与行及相应的列与列对调, 化为主对角型分块矩阵, 且对角线上每一个子块都是全 $1$ 方阵.

### 等价类与划分

**定义**: 设 $A$ 为任意集合且 $\Pi\sube\mathscr{P}(A)$ . 如果 $\Pi$ 满足:
1. $S\in\Pi\rightarrow S\neq\varnothing$
2. $\cup\Pi=A$
3. $\forall S_1\forall S_2(S_1\in\Pi\land S_2\in\Pi\land S_1\cap S_2\neq \varnothing\rightarrow S_1=S_2)$

则称 $\Pi$ 为 $A$ 的划分.

**定义**: 设 $R$ 为集合 $A$ 上的等价关系. 对每个 $x\in A$ , 令

$$
[x]_R=\lbrace y\mid y\in A\land xRy\rbrace
$$

则称 $[x]_R$ 为 $x$ 关于 $R$ 的等价类. 当不强调 $R$ 时, 就把 $[x]_R$ 简记为 $[x]$ , 并称为 $x$ 的等价类.

**定义**: 设 $R$ 为集合 $A$ 上的等价关系. 称集合 $\lbrace[x]_R\mid x\in A\rbrace$ 为 $A$ 关于 $R$ 的商集, 并记为 $A/R$ .

**定理**: 若 $R$ 为集合 $A$ 上的等价关系, 则 $\Pi_R=A/R=\lbrace[x]_R\mid x\in A\rbrace$ 为 $A$ 的划分.

**定理**: 若 $\Pi$ 为集合 $A$ 的划分. 若令:

$$
R_{\Pi}=\lbrace<x,y>\mid \exist S(S\in\Pi\land x\in S\land y\in S)\rbrace
$$

则 $R_{\Pi}$ 为 $A$ 上的等价关系, 且 $A/R_{\Pi}=\Pi$ .

## 序关系

### 拟序与半序及基本定理

**定义**: 设 $R$ 为集合 $A$ 上的二元关系.
1. 如果 $R$ 是反自反和传递的, 则称 $R$ 为 $A$ 上的拟序关系, 简称**拟序**, 并称 $<A,R>$ 为拟序结构.
   > 可以证明拟序一定是**反对称的**.
2. 如果 $R$ 是自反的, 反对称的, 传递的, 则称 $R$ 为 $A$ 上的半序关系, 简称**半序**, 并称 $<A,R>$ 为半序结构.
   > 半序, 又称偏序, 部分序.

**定理**: 设 $R$ 为集合 $A$ 上的二元关系.
1. 若 $R$ 为 $A$ 上的拟序, 则 $r(R)$ 为 $A$ 上的半序.
2. 若 $R$ 为 $A$ 上的半序, 则 $R-\bm{I}_A$ 为 $A$ 上的拟序.

我们常用" $\le$ "表示半序, 用" $<$ "表示拟序. 则有 $\le =r(<)$ .

### 覆盖与哈斯图

**定义**: 设 $R$ 为集合 $A$ 上的半序且 $a\in A$ . 如果 $b\in A$ 满足:
1. $b\neq a\land aRb$
2. $\exist x(x\in A\land aRx\land xRb)\rightarrow x=a\lor x=b$

则称 $b$ 为 $a$ 关于 $R$ 的覆盖. 在不强调 $R$ 关系时, 也简称" $b$ 为 $a$ 的覆盖".

若二元关系是半序, 可以简化其关系图, 即哈斯图.

**定义**: 设 $R$ 为非空有限集 $A$ 上的半序. 如果无向图 $H_R$ 满足:
1. $H_R$ 仅以 $A$ 的所有元素为结点.
2. 若 $b\in A$ 为 $a\in A$ 关于 $R$ 的覆盖, 则结点 $a$ 在 $H_R$ 中就处于结点 $b$ 的下一级, 且有一条连接 $a$ 和 $b$ 的无向边.

就称 $H_R$ 为 $R$ 的哈斯图.

**定义**: 如果集合 $A$ 上的半序 $R$ 满足: $\forall x\forall y(x\in A\land y\in A\rightarrow xRy\lor yRx)$ , 则称 $R$ 为 $A$ 上的全序关系, 简称**全序**, 并称 $<A,R>$ 为链或全序结构.

### 界, 确界, 极元, 最元

**定义**: 设 $\le$ 为非空集 $A$ 上的半序且 $S$ 为 $A$ 的非空子集.
1. 若 $\exist a(a\in A\land\forall x(x\in S\rightarrow x\le a))$ (或 $\exist a(a\in A\land\forall x(x\in S\rightarrow a\le x))$ ), 则称 $S$ 有上(或下)界, 称 $a$ 为 $S$ 的一个上(或下)界.
2. 若有 $S$ 的上(或下)界 $a$ , 使得对于 $S$ 的每一个上(或下)界 $y$ 皆有 $a\le y$ (或 $y\le a$ ), 就称 $S$ 有上(或下)确界, 并称 $a$ 为 $S$ 的一个上(或下)确界, 记为 $a=\sup S$ (或 $a=\inf S$ ).
3. 若 $\exist a(a\in S\land \forall x(x\in S\land a\le x\rightarrow x=a))$ (或 $\exist a(a\in S\land \forall x(x\in S\land x\le a\rightarrow x=a))$ ), 就称 $S$ 有极大(或小)元, 并称 $a$ 为 $S$ 的一个极大(或小)元.
4. 若 $\exist a(a\in S\land\forall x(x\in S\rightarrow x\le a))$ (或 $\exist a(a\in S\land\forall x(x\in S\rightarrow a\le x))$ ), 就称 $S$ 有最大(或小)元, 并称 $a$ 为 $S$ 的一个最大(或小)元.

**定理**: 设 $\le$ 为非空集 $A$ 上的半序, $S$ 是 $A$ 的非空子集.
1. 若 $S$ 的上(或下)确界存在, 则它一定是唯一的.
2. 若 $S$ 的最大(或小)元存在, 则它一定是唯一的.

### 良序及基本定理

**定义**: 设 $R$ 为集合 $A$ 上的半序. 如果 $A$ 的每一个非空子集都有最小元, 就称 $R$ 为 $A$ 上的良序关系, 简称良序, 并称 $<A,R>$ 为良序结构.

**定理**: 设 $\le$ 为自然数集 $\bm{N}$ 上的数的小于等于关系, $<\bm{N},\le>$ 是良序结构.

**定理**: 若 $\le$ 为集合 $A$ 上的半序, 则 $\le$ 为 $A$ 上的良序的充要条件为:
1. $\le$ 为 $A$ 上的全序.
2. $A$ 的每一个非空子集都有极小元.

**定理**: 若 $<A,\le>$ 为全序结构, 则 $<A,\le>$ 为良序结构的充要条件为: 不存在 $A$ 中元素的无穷序列 $a_0,a_1,a_2,\dotsm$ 使得 $\forall i(i\in\bm{N}\rightarrow a_{i+1}\le a_i)$
> * 证必要性. 假定存在 $A$ 中元素的无穷序列 $a_0,a_1,a_2,\dotsm$ 使得对每个 $i\in\bm{N}$ 皆有 $a_{i+1}\le a_i$ . 令 $S=\lbrace a_i\mid i\in\bm{N}\rbrace$ 则 $S$ 为 $A$ 的非空子集, 且 $S$ 没有最小元. 因此 $<A,\le>$ 不是良序结构.
> * 证充分性. 假定 $<A,\le>$ 不是良序结构, 则必有 $A$ 的一个非空子集 $S$ 没有最小元. 任取 $a_0\in S$ , 因为 $a_0$ 不是 $S$ 的最小元且 $\le$ 为 $A$ 上的全序, 因而必有 $a_1\in S$ 使得 $a_1\le a_0$ . 以此类推, 便得到了 $A$ 中的无穷序列:
>   
>   $$
>   a_n\le a_{n-1}\le a_{n-2}\le\dotsm\le a_0
>   $$
> 
>   使得对于每个 $i\in\bm{N}$ 都有 $a_{i+1}\le a_i$ .
> 
> 该定理是验证程序终止性的理论基础.