---	
layout:     post	
title:      『Linear Algebra』 Homogeneous Linear Recurrence Sequence	
subtitle:   『线性代数』 齐次线性递推序列    
date:       2021-02-15	   
author:     Coekjan 
header-img: img/post-bg-LA.jpg	
catalog:    true	
katex:    true    
tags:	
    - Linear Algebra  
---

## 齐次线性递推关系

若一个序列 $\lbrace a_n\rbrace,n\in\mathbf{N}^+$ 满足递推关系:

$$
a_n=\left\{\begin{aligned}
    &a_1&&,n=1\\
    &\vdots&&\vdots\\
    &a_k&&,n=k\\
    &x_1a_{n-1}+x_2a_{n-2}+\dotsm+x_ka_{n-k}&&,n>k
\end{aligned}\right.\quad(\forall i \in \{1,2,\dotsm,k\}, x_i\neq0)
$$

则称这个序列是**齐次线性递推序列**, 同时称这个序列满足**齐次线性递推关系**. 其中, 把 $k$ 称为这个递推关系的**阶数**, $a_1,\dotsm,a_k$ 都是已知的常数.

## 齐次线性递推的初等解法

### 一阶齐次线性递推序列

考虑一阶递推关系:

$$
a_n=\left\{\begin{aligned}
    &a_1,&&n=1\\
    &xa_{n-1},&&n>1
\end{aligned}\right.\quad(x\neq0)
$$

显然可以进行直接迭代求解通项公式:

$$
\forall n>1,a_n=xa_{n-1}=\dotsm=x^{n-1}a_1
$$

因此可以得到通项公式:

$$
a_n=a_1x^{n-1},n\in\mathbf{N}^+
$$

### 二阶齐次线性递推序列

考虑二阶递推关系:

$$
a_n=\left\{\begin{aligned}
    &a_1&&,n=1\\
    &a_2&&,n=2\\
    &x_1a_{n-1}+x_2a_{n-2}&&,n>2
\end{aligned}\right.\quad(x_1,x_2\neq0)
$$

#### 特征方程法求解通项公式

考虑把 $a_n=x_1a_{n-1}+x_2a_{n-2}$ 配凑为如下形式:

$$
a_n-\lambda_1a_{n-1}=\lambda_2(a_{n-1}-\lambda_1a_{n-2})
$$

将该形式展开, 与递推式比对:

$$
\begin{aligned}
    a_n=x_1a_{n-1}+x_2a_{n-2}&\Leftrightarrow a_n=(\lambda_1+\lambda_2)a_{n-1}-\lambda_1\lambda_2a_{n-2}\\
    \Rightarrow&\left\{\begin{aligned}
        x_1&=\lambda_1+\lambda_2\\
        -x_2&=\lambda_1\cdot \lambda_2
    \end{aligned}\right.
\end{aligned}
$$

根据韦达定理, $\lambda_1,\lambda_2$ 正是方程 $\lambda^2-x_1\lambda-x_2=0$ 的两根.

称这个方程为递推关系 $a_n-x_1a_{n-1}-x_2a_{n-2}=0$ 的**特征方程**, $\lambda_1,\lambda_2$ 为**特征根**. 解特征方程即可得到 $\lambda_1,\lambda_2$ 的值. 由于 $x_2=-\lambda_1\lambda_2\neq0$ , 因此 $\lambda_1,\lambda_2\neq0$ .

迭代得:

$$
a_n-\lambda_1a_{n-1}=\lambda_2^{n-2}(a_2-\lambda_1a_1),n>2
$$

因此:

$$
\begin{aligned}
    a_n&=(a_2-\lambda_1a_1)\lambda_2^{n-2}+\lambda_1a_{n-1}\\
    \Rightarrow\frac{a_n}{\lambda_1^n}&=\frac{a_2-\lambda_1a_1}{\lambda_2^2}\left(\frac{\lambda_2}{\lambda_1}\right)^n+\frac{a_{n-1}}{\lambda_1^{n-1}}\\
    &=\dotsm=\frac{a_2-\lambda_1a_1}{\lambda_2^2}\left[\left(\frac{\lambda_2}{\lambda_1}\right)^n+\left(\frac{\lambda_2}{\lambda_1}\right)^{n-1}+\dotsm+\left(\frac{\lambda_2}{\lambda_1}\right)^3\right]+\frac{a_2}{\lambda_1^2}\\
    &=\left\{\begin{aligned}
        &\frac{a_2-\lambda a_1}{\lambda^2}(n-2)+\frac{a_2}{\lambda^2}&&,\lambda_1=\lambda_2=\lambda\\
        &\frac{a_2-\lambda_1a_1}{\lambda_2}\frac{\displaystyle\left(\frac{\lambda_2}{\lambda_1}\right)^2-\left(\frac{\lambda_2}{\lambda_1}\right)^n}{\displaystyle \lambda_1-\lambda_2}+\frac{a_2}{\lambda_1^2}&&,\lambda_1\neq \lambda_2
    \end{aligned}\right.\\
    \dotsm\Rightarrow a_n&=\left\{\begin{aligned}
        &\left[\left(\frac{a_2}{\lambda}-a_1\right)n+2a_1-\frac{a_2}{\lambda}\right]\lambda^{n-1}&&,\lambda_1=\lambda_2=\lambda\\
        &\frac{a_2-\lambda_2a_1}{\lambda_1-\lambda_2}\lambda_1^{n-1}-\frac{a_2-\lambda_1a_1}{\lambda_1-\lambda_2}\lambda_2^{n-1}&&,\lambda_1\neq \lambda_2
    \end{aligned}\right.
\end{aligned}
$$

这说明:

$$
a_n=\left\{\begin{aligned}
    &(A_1n+B_1)\lambda^{n-1}&&,\lambda_1=\lambda_2=\lambda\\
    &A_2\lambda_1^{n-1}+B_2\lambda_2^{n-1}&&,\lambda_1\neq\lambda_2
\end{aligned}\right.
$$

只需求出特征根 $\lambda_1, \lambda_2$ , 选择适当的形式, 使用待定系数法即可求解通项公式.

## 矩阵幂法求解齐次线性递推

构造矩阵:

$$
\bm{A}=\begin{bmatrix}
    x_1 & x_2 & \dotsm & x_{k-1} & x_k \\
    1 & 0 & \dotsm & 0 & 0 \\
    0 & 1 & \dotsm & 0 & 0 \\
    \vdots & \vdots & \ddots & \vdots & \vdots \\
    0 & 0 & \dotsm & 1 & 0
\end{bmatrix}_{k\times k}
$$

此时有:

$$
\begin{bmatrix}
    a_n \\ a_{n-1} \\ \vdots \\ a_{n-k+1}
\end{bmatrix}=\bm{A}\begin{bmatrix}
    a_{n-1} \\ a_{n-2} \\ \vdots \\ a_{n-k}
\end{bmatrix}=\bm{A}^{n-k}\begin{bmatrix}
    a_k \\ a_{k-1} \\ \vdots \\ a_1
\end{bmatrix}
$$

因此关键是求 $\bm{A}^{n-k}$ . 一般来讲, 求矩阵的高次幂有以下技巧:
* [矩阵快速幂](#矩阵快速幂) - 算法
* [二项式展开求矩阵幂](#二项式展开求矩阵幂)
* [相似对角化](#相似对角化)

### 矩阵快速幂

由二进制理论, 任意一个正整数 $m$ , 都能表示为:

$$
m=\sum_{i=0}^\infty w_i2^i,\quad w_i=0,1
$$

因此指数部分可以写作二进制形式吗, 例如:

$$
6^9=6^{2^3+2^0}=6^{1001_{(2)}}
$$

程序上, 只需不断作平方运算, 根据二进制位的 $0/1$ 取舍即可, 下面给出伪代码:

```pascal
FUNCTION FAST_POW (base, idx) : // INPUT
    res = 1
    WHILE idx != 0 :
        IF idx & 1 == 1 :
            res = res * base
        base = base * base
        idx = idx >> 1
    EXIT res // OUTPUT
```

矩阵快速幂是相同的原理, 只需把 `base` 的换成底矩阵, `res` 初始化为单位阵即可. 整数快速幂的时间复杂度为 $\mathcal{O}(\log_2n)$ , 其中 $n$ 是指数; 而不加优化的矩阵快速幂时间复杂度为 $\mathcal{O}(k^3\log_2n)$ , 其中 $k$ 是矩阵的阶数, $n$ 是指数.

#### Hamilton-Cayley定理优化线性递推

##### Hamilton-Cayley定理

定义 $k$ 阶方阵 $\bm{M}$ 的**特征多项式为** $f(\lambda)=\det(\lambda \bm{I} - \bm{M})$ ; 使得 $f(\lambda)=0$ 的 $\lambda$ 称为 $\bm{M}$ 的**特征值**.

**Hamilton-Cayley定理**断言 $f(\bm{M})=\bm{O}$ .

##### 优化矩阵快速幂

有定理保证 $k$ 阶线性递推的矩阵 $\bm{A}$ 的特征多项式:

$$
f(\lambda)=\det(\lambda\bm{I}-\bm{A})=\lambda^k-\sum_{i=1}^kx_i\lambda^{k-i}
$$

对于任意的 $n$ , 可将 $\bm{A}^n$ 分解:

$$
\bm{A}^n=Q(\bm{A})f(\bm{A})+R(\bm{A})
$$

由Hamilton-Cayley定理即得:

$$
f(\bm{A})=\bm{A}^k-\sum_{i=1}^kx_i\bm{A}^{k-i}=\bm{O}
$$

因此: $\bm{A}^n=R(\bm{A})$ , 此时多项式 $R$ 的次数小于 $k$ . 另外, 可使用多项式取模求 $R$ , 再用矩阵快速幂计算 $R(\bm{A})$ . 若多项式取模使用快速幂, 则总时间复杂度为 $\mathcal{O}(k^2\log_2n)$ ; 若使用快速傅里叶变换求多项式乘法和取模, 则时间复杂度更优: $\mathcal{O}(k\log_2k\log_2n)$ .

### 二项式展开求矩阵幂

若 $k$ 阶方阵 $\bm{A}$ 可以分解为 $\bm{A}=\bm{P}+\bm{Q}$ , 且 $\bm{PQ}=\bm{QP}$ , 并且 $\bm{Q}$ 的高次幂容易计算(如幂零矩阵), 则可以使用二项式展开简化矩阵的幂运算:

$$
\bm{A}^n=(\bm{P}+\bm{Q})^n=\sum_{i=0}^n\dbinom{n}{i}\bm{P}^i\bm{Q}^{n-i}
$$

### 相似对角化

若 $k$ 阶方阵 $\bm{A}$ 可以相似到对角型, 即存在可逆矩阵 $\bm{P}$ 与对角型 $\bm{D}$ , 使得: $\bm{A}=\bm{P}^{-1}\bm{DP}$ , 其中:

$$
\bm{D}=\begin{bmatrix}
    \lambda_1 \\
     & \lambda_2 \\
     &  & \ddots \\
     &  &  & \lambda_k
\end{bmatrix},\bm{P}=[\bm{X}_1,\bm{X}_2,\dotsm,\bm{X}_k]\quad(\bm{AX}_i=\lambda_i\bm{X}_i)
$$

特征向量 $\bm{X}_1,\bm{X}_2,\dotsm,\bm{X}_k$ 应线性无关. 则可以简化 $\bm{A}^n$ 计算:

$$
\bm{A}^n=(\bm{P}^{-1}\bm{DP})^n=\bm{P}^{-1}\bm{D}^n\bm{P}
$$

#### 相似到Jordan标准型(准对角化)

并非任意矩阵 $\bm{A}$ 都能进行相似对角化, 但是都能进行准对角化: 存在可逆矩阵 $\bm{P}$ 和Jordan标准型 $\bm{J}$ 使得 $\bm{A}=\bm{P}^{-1}\bm{JP}$ , 其中:

$$
\bm{J}=\begin{bmatrix}
    \bm{J}_1 \\
     & \bm{J}_2 \\
     &  & \ddots \\
     &  &  & \bm{J}_m
\end{bmatrix}_{k\times k},\bm{J}_i=\begin{bmatrix}
    \lambda_i & 1 \\
     & \lambda_i & \ddots \\
     &  & \ddots & 1 \\
     &  &  & \lambda_i
\end{bmatrix}
$$

不加证明地指出:

$$
\bm{J}^n=\begin{bmatrix}
    \bm{J}_1^n \\
     & \bm{J}_2^n \\
     &  & \ddots \\
     &  &  & \bm{J}_m^n
\end{bmatrix}_{k\times k}
$$

其中:

$$
\bm{J}_i=\lambda_i\bm{I}+\bm{Q},\quad\bm{Q}=\begin{bmatrix}
    0 & 1 \\
     & 0 & 1 \\
     &  & 0 & \ddots \\
     &  &  & \ddots & 1 \\
     &  &  &  & 0
\end{bmatrix}
$$

把 $\bm{J}_i$ 分解为数量阵与幂零矩阵, 其 $n$ 次幂可以通过二项式展开求解.

最后, 利用 $\bm{A}^n=\bm{P}^{-1}\bm{J}^n\bm{P}$ 即可求得 $\bm{A}^n$ .

## 齐次线性递推关系的通解

### 引理

**引理内容**: 若多项式 $f(x)=x^n+c_{n-1}x^{n-1}+\dotsm+c_1x+c_0$ 有 $k$ 重根 $x_{1,2,\dotsm,k}=x_1=x_2=\dotsm=x_k\neq0$ 则:

$$
\left\{\begin{aligned}
    x_{1,2,\dotsm,k}^0\cdot f^{(0)}(x_{1,2,\dotsm,k})&=0\\
    x_{1,2,\dotsm,k}^1\cdot f^{(1)}(x_{1,2,\dotsm,k})&=0\\
    &\vdots\\
    x_{1,2,\dotsm,k}^{k-1}\cdot f^{(k-1)}(x_{1,2,\dotsm,k})&=0
\end{aligned}\right.\qquad\left(f^{(n)}=\frac{\operatorname{d}^n}{\operatorname{d}x^n}f\right)
$$

**证明**:

根据代数基本定理, 多项式 $f$ 有 $n$ 个复根: $x_1,x_2,\dotsm,x_k,\dotsm,x_n$ 其中:

$$
x_1=x_2=\dotsm=x_k=x_{1,2,\dotsm,k}
$$

从而 $f(x)$ 可以写作:

$$
f(x)=(x-x_{1,2,\dotsm,k})^k\prod_{i=k+1}^n(x-x_i)
$$

求导, 易得:

$$
\left\{\begin{aligned}
    f^{(0)}(x_{1,2,\dotsm,k})&=0\\
    f^{(1)}(x_{1,2,\dotsm,k})&=0\\
    &\vdots\\
    f^{(k-1)}(x_{1,2,\dotsm,k})&=0
\end{aligned}\right.
$$

对于第 $i$ 个等式, 左边添加 $x_{1,2,\dotsm,k}^{i-1}$ 即得需证明的等式组.

**推论**: 若多项式 $f(x)$ 有多组重根, 则对于每一组重根, 都有引理成立.

### 齐次线性递推的通解

若一个序列 $\lbrace a_n\rbrace,n\in\mathbf{N}^+$ 满足递推关系:

$$
a_n=x_1a_{n-1}+x_2a_{n-2}+\dotsm+x_ka_{n-k},n>k
$$

考虑取 $m(m>k)$ 个复数组成的序列 $\bm{\alpha}=(\alpha_1,\alpha_2,\dotsm,\alpha_m)$ . 此时有 $\bm{\alpha}\in\mathbf{C}^{m}$ . 而满足该递推关系的序列构成了子空间 $\bm{W}$ , 显然该子空间对加法与数乘封闭. 当给定了序列的前 $k$ 项: $\alpha_1,\alpha_2,\dotsm,\alpha_k$ , 由递推关系即可完全确定 $\bm\alpha$ . 因此可以把 $(\alpha_1,\alpha_2,\dotsm,\alpha_k)$ 作为序列 $\bm\alpha$ 的"坐标". 也就是说, 子空间 $\bm{W}$ 的维数为 $k$ .

任意等比序列 $\bm\beta=(\beta_1,\beta_2,\dotsm,\beta_m)$ 有通项公式: $\beta_n=\beta_1q^{n-1}(1\le n\le m)$ . 代入递推关系即得:

$$
q^{n-1}=\sum_{i=1}^kx_iq^{n-i-1}
$$

由于 $q\neq0$ , 因此可以整理为:

$$
q^k=\sum_{i=1}^kx_iq^{k-i}
$$

事实上, 这个关于 $q$ 的方程就是该递推关系的**特征方程**.

#### 特征方程无重根

选取特征方程的 $k$ 个根作为公比, 得到公比不同(线性无关)的等比序列: $\bm{\alpha}_i=(1,q_i,q_i^2,\dotsm,q_i^{m-1})(i=1,2,\dotsm,k)$ , 此时满足该递推关系的序列 $\bm{a}=(a_1,a_2,\dotsm,a_m)$ 可以写作这些等比序列的线性组合:

$$
\bm{a}=\sum_{i=1}^kC_i\bm{\alpha}_i
$$

可以得到递推关系的通解:

$$
a_n=\sum_{i=1}^kC_iq_i^{n-1}
$$

#### 特征方程有重根

假设有一组 $w$ 重根 $q_i,q_{i+1},\dotsm,q_{i+w-1}$ 记作 $\mathscr{Q}$ , 根据引理:

$$
\left\{\begin{aligned}
    \mathscr{Q}^k&=\sum_{i=1}^kx_i\mathscr{Q}^{k-i}\\
    k^1\mathscr{Q}^k&=\sum_{i=1}^kx_ik^1\mathscr{Q}^{k-i}\\
    \vdots&\\
    k^{w-1}\mathscr{Q}^k&=\sum_{i=1}^kx_ik^{w-1}\mathscr{Q}^{k-i}
\end{aligned}\right.
$$

因此除了等比序列 $\mathscr{Q}^n$ 是该递推关系的通解, 还有 $n\mathscr{Q}^n,n^2\mathscr{Q}^n,\dotsm,n^{w-1}\mathscr{Q}^n$ 也是该递推关系的通解. 此时, 针对该 $w$ 重根, 可选取线性无关的序列:

$$
\begin{aligned}
    \bm{\alpha}_{i}&=(1,\mathscr{Q},\mathscr{Q}^2,\dotsm,\mathscr{Q}^{m-1}) \\
    \bm{\alpha}_{i+1}&=(0,\mathscr{Q},2\mathscr{Q}^2,\dotsm,(m-1)\mathscr{Q}^{m-1}) \\
    \vdots \\
    \bm{\alpha}_{i+w-1}&=(0,\mathscr{Q},2^{w-1}\mathscr{Q}^2.\dotsm,(m-1)^{w-1}\mathscr{Q}^{m-1})
\end{aligned}
$$

对于非重根 $q_j$ , 选取 $\bm{\alpha}_j=(1,q_j,q_j^2,\dotsm,q_j^{m-1})$ 即可.

那么, 满足该递推关系的序列即可写作这些序列的线性组合:

$$
\bm{a}=\sum_{j=1}^kC_j\bm{\alpha}_j
$$

类似地, 得到通解:

$$
a_n=\sum_{j\notin\lparen i,i+w-1\rbrack}C_jq_j^{n-1}+\left(\sum_{j\in\lparen i,i+w-1\rbrack}C_jn^{j-i}\right)\mathscr{Q}^{n-1}
$$

多组重根的情形是类似的, 此处不再赘述.

> 此处的结论与[二阶齐次线性递推序列](#二阶齐次线性递推序列)的讨论结果是一致的.