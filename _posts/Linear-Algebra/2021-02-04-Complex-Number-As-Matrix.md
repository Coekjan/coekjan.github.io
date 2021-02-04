---	
layout:     post	
title:      『Linear Algebra』 Complex Number - As Matrix	
subtitle:   『线性代数』 复数 - 作为矩阵    
date:       2021-02-04	   
author:     Coekjan 
header-img: img/post-bg-LA.jpg	
catalog:    true	
katex:    true    
tags:	
    - Linear Algebra  
---

## 复数的矩阵表示

引入实数 $1$ 对应的实部单位阵 $\bm{I}$ :

$$
\bm{I}=\begin{pmatrix}
    1 & 0 \\
    0 & 1
\end{pmatrix}
$$

引入虚数 $i$ 对应的虚部单位阵 $\bm{J}$ :

$$
\bm{J}=\begin{pmatrix}
    0 & -1 \\
    1 & 0
\end{pmatrix}
$$

> 矩阵 $\bm{J}$ 的线性变换含义是逆时针旋转 $90^\circ$ .

称复数 $z$ 对应的矩阵为 $\bm{Z}$ :

$$
\bm{Z}=\begin{pmatrix}
    a & -b \\
    b & a
\end{pmatrix} = a\cdot\bm{I} + b\cdot\bm{J}
$$

其中 $a$ 为 $z$ 的实部, $b$ 为 $z$ 的虚部.

令集合:

$$
\bm{\Xi}=\left\{\begin{pmatrix}
    a & -b \\
    b & a
\end{pmatrix} \middle\vert a,b\in\bf{R} \right\}
$$

本质上, 构造了**双射** $\zeta : \bf{C}\rightarrow\Xi$ .

### 复数基本运算的矩阵表示

考虑复数 $z_1=a_1+b_1i, z_2=a_2+b_2i$ , 矩阵 $\bm{Z}_1=\zeta(z_1),\bm{Z}_2=\zeta(z_2)$ , 下面给出其运算与矩阵的对应形式.

复数运算 | 矩阵形式
:-----: | :------:
$z_1+z_2=(a_1+a_2)+(b_1+b_2)i$ | $\bm{Z}_1+\bm{Z}_2=(a_1+a_2)\bm{I}+(b_1+b_2)\bm{J}$
$kz_1=(ka_1)+(kb_1)i$ | $k\bm{Z}_1=(ka_1)\bm{I}+(kb_1)\bm{J}$
$z_1z_2=(a_1a_2-b_1b_2)+(a_1b_2+a_2b_1)i$ | $\bm{Z}_1\bm{Z}_2=(a_1a_2-b_1b_2)\bm{I}+(a_1b_2+a_2b_1)\bm{J}$
$\vert z_1\vert=a_1^2+b_1^2$ | $\det\bm{Z}_1=a_1^2+b_1^2$
$\overline{z_1}=a_1-b_1i$ | $\bm{Z}_1^T=a_1\bm{I}-b_1\bm{J}$
$z_1^{-1}=\vert z_1\vert^{-1}\cdot\overline{z_1}(z_1\neq0)$ | $\bm{Z}_1^{-1}=(\det \bm{Z}_1)^{-1}\cdot\bm{Z}_1^T(\bm{Z}_1\neq\bm{O})$

易证 $\bm{\Xi}$ 内的矩阵 $\bm{Z}_1,\bm{Z}_2$ 满足乘法交换律: $\bm{Z}_1\bm{Z}_2=\bm{Z}_2\bm{Z}_1$ . 这与复数本身的乘法交换律相对应.

也易证 $\bm{\Xi}$ 对加法, 数乘, 乘法均封闭.

### 从矩阵看复数乘法的线性变换意义

对于一个矩阵 $\bm{Z}=a\bm{I}+b\bm{J}$ 可作变化:

$$
\begin{aligned}
    \bm{Z}=&\sqrt{\det\bm{Z}}\begin{pmatrix}
        \displaystyle\frac{a}{\sqrt{a^2+b^2}} & \displaystyle-\frac{b}{\sqrt{a^2+b^2}} \\
        \displaystyle\frac{b}{\sqrt{a^2+b^2}} & \displaystyle\frac{a}{\sqrt{a^2+b^2}}
    \end{pmatrix}\\
    \xlongequal{r:=\sqrt{\det\bm{Z}}}&\underbrace{\begin{pmatrix}
        r & 0 \\
        0 & r
    \end{pmatrix}}_{\text{Scaling}}
    \underbrace{\begin{pmatrix}
        \cos\theta & -\sin\theta \\
        \sin\theta & \cos\theta
    \end{pmatrix}}_{\text{Rotation}}=r\cos\theta\cdot\bm{I}+r\sin\theta\cdot\bm{J}
\end{aligned}
$$

因此复数乘法在线性变换中可解释为旋转与伸缩. 显然, 上式中伸缩矩阵与旋转矩阵是可交换的, 也就是说旋转与伸缩无先后关系. **实质上, $r$ 即为复数的模, $\theta$ 即为复数的辐角**.

特别地, 矩阵 $\bm{J}$ 代表的线性变换为逆时针旋转 $90^\circ$ .

另外, 对于多个复数乘法的情形, 可用上式进行迅速简化计算:

$$
\prod_{k=1}^n\bm{Z}_k=\begin{pmatrix}
    \displaystyle\prod_{k=1}^nr_k & 0 \\
    0 & \displaystyle\prod_{k=1}^nr_k
\end{pmatrix}\begin{pmatrix}
    \displaystyle\cos\sum_{k=1}^n\theta_k & -\displaystyle\sin\sum_{k=1}^n\theta_k \\
    \displaystyle\sin\sum_{k=1}^n\theta_k & \displaystyle\cos\sum_{k=1}^n\theta_k
\end{pmatrix}
$$

类似, 复数的幂次方运算也可如上式进行迅速简化:

$$
\bm{Z}^n=\begin{pmatrix}
    r^n & 0 \\
    0 & r^n
\end{pmatrix}\begin{pmatrix}
    \displaystyle\cos n\theta & -\displaystyle\sin n\theta \\
    \displaystyle\sin n\theta & \displaystyle\cos n\theta
\end{pmatrix}
$$

## 复变函数的矩阵表示

函数 $f:\bf{C}\rightarrow\bf{C}$ 称为复变函数. 引入 $\mathscr{F}:\bf{\Xi}\rightarrow\bf{\Xi}$ 称为复变函数 $f$ 的矩阵表示.

### 复变函数极限的矩阵表示

$$
\forall\varepsilon>0,\exist\delta>0\quad\text{s.t.}\quad\forall z \quad \vert z-z_0\vert<\delta\rightarrow\vert f(z)-w\vert<\varepsilon
$$

则称 $f$ 在 $z_0$ 处的极限为 $w$ .

相应的矩阵表示为:

$$
\forall\varepsilon>0,\exist\delta>0\quad\text{s.t.}\quad\forall\bm{Z} \quad \det(\bm{Z}-\bm{Z}_0)<\delta\rightarrow\det(\mathscr{F}(\bm{Z})-\bm{W})<\varepsilon
$$

则称 $\mathscr{F}$ 在 $\bm{Z}_0$ 处的极限为 $\bm{W}$ .

### 复变函数微分的矩阵表示

若存在复数 $f^{\prime}(z_0)$ 使得 $z_0$ 附近的任意点 $z=z_0+\Delta z$ 都有:

$$
f(z)-f(z_0)=f'(z_0)\Delta z+r(\Delta z),\quad\lim_{\vert\Delta z\vert\rightarrow0}\frac{\vert r(\Delta z)\vert}{\vert\Delta z\vert}=0
$$

则称 $f^{\prime}(z_0)\Delta z$ 为 $f$ 在 $z_0$ 处的微分.

相应的矩阵表示为: 若存在 $2\times2$ 方阵 $\mathscr{F}^{\prime}(\bm{Z}_0)$ 使得任意的矩阵 $\bm{Z}=\bm{Z}_0+\bm{\Delta Z}$ 都有:

$$
\mathscr{F}(\bm{Z})-\mathscr{F}(\bm{Z}_0)=\mathscr{F}'(\bm{Z}_0)\bm{\Delta Z}+\bm{r}(\bm{\Delta Z}),\quad\lim_{\det\bm{\Delta Z}\rightarrow0}\frac{\det\bm{r}(\bm{\Delta Z})}{\det\bm{\Delta Z}}=0
$$

则称 $\mathscr{F}^{\prime}(\bm{Z}_0)\bm{\Delta Z}$ 为 $\mathscr{F}$ 在 $\bm{Z}_0$ 处的微分.

> 由于集合 $\bm{\Xi}$ 对加法与乘法封闭, 因此 $\mathscr{F}^{\prime}(\bm{Z}_0)\in\bm{\Xi}$ .

#### 矩阵表示下复变函数微分的求解

命复变函数 $f(z)=u(x,y)+v(x,y)i$ , 其中 $x,y$ 分别为 $z$ 的实部与虚部, 则 $f$ 对应的矩阵表示为:

$$
\mathscr{F}(\bm{Z})=\begin{pmatrix}
    u(x,y) & -v(x,y) \\
    v(x,y) & u(x,y)
\end{pmatrix}
$$

根据微分的矩阵表示:

$$
\begin{aligned}
    &\begin{pmatrix}
        u(x_0+\Delta x,y_0+\Delta y) & -v(x_0+\Delta x,y_0+\Delta y) \\
        v(x_0+\Delta x,y_0+\Delta y) & u(x_0+\Delta x,y_0+\Delta y)
    \end{pmatrix}-
    \begin{pmatrix}
        u(x_0,y_0) & -v(x_0,y_0) \\
        v(x_0,y_0) & u(x_0,y_0)
    \end{pmatrix}\\
    =&\mathscr{F}'(\bm{Z}_0)\cdot\begin{pmatrix}
        \Delta x & -\Delta y \\
        \Delta y & \Delta x
    \end{pmatrix} + \bm{r}(\bm{\Delta Z})
\end{aligned}
$$

待定 $\mathscr{F}'(\bm{Z}_0)$ 为

$$
\mathscr{F}'(\bm{Z}_0)=\begin{pmatrix}
    \Re & -\Im \\
    \Im & \Re
\end{pmatrix}
$$

记 $\Delta u:=u(x_0+\Delta x,y_0+\Delta y)-u(x_0,y_0),\:\Delta v:=v(x_0+\Delta x,y_0+\Delta y)-v(x_0,y_0)$ , 得:

$$
\begin{aligned}
    \begin{pmatrix}
        \Delta u & -\Delta v \\
        \Delta v & \Delta u
    \end{pmatrix}&=\begin{pmatrix}
        \Re & -\Im \\
        \Im & \Re
    \end{pmatrix}\begin{pmatrix}
        \Delta x & -\Delta y \\
        \Delta y & \Delta x
    \end{pmatrix}+\bm{r}(\bm{\Delta Z})\\
    &=\begin{pmatrix}
        \Re\Delta x-\Im\Delta y & -(\Re\Delta y+\Im\Delta x) \\
        \Re\Delta y+\Im\Delta x & \Re\Delta x-\Im\Delta y
    \end{pmatrix}+\bm{r}(\bm{\Delta Z})
\end{aligned}
$$

比对矩阵相应的项, 可得:

$$
\left\{\begin{aligned}
    \Delta u&=\Re\Delta x-\Im\Delta y+r_u(\Delta x, \Delta y)\quad\frac{r_u(\Delta x, \Delta y)}{\sqrt{\Delta x^2+\Delta y^2}}\rightarrow0\\
    \Delta v&=\Re\Delta y+\Im\Delta x+r_v(\Delta x, \Delta y)\quad\frac{r_v(\Delta x, \Delta y)}{\sqrt{\Delta x^2+\Delta y^2}}\rightarrow0
\end{aligned}\right.\quad (\sqrt{\Delta x^2+\Delta y^2}\rightarrow0)
$$

令 $\bm{\Delta Z}\rightarrow\bm{O}\Leftrightarrow\Delta x\rightarrow0, \Delta y\rightarrow0$ , 则有:

$$
\left\{\begin{aligned}
    \operatorname{d}u&=\Re\operatorname{d}x-\Im\operatorname{d}y\\
    \operatorname{d}v&=\Re\operatorname{d}y+\Im\operatorname{d}x
\end{aligned}\right.
$$

由二元函数微分的性质, 得到:

$$
\left.\frac{\partial u}{\partial x}\right\vert_{x=x_0}=\Re=\left.\frac{\partial v}{\partial y}\right\vert_{y=y_0}\qquad\left.-\frac{\partial u}{\partial y}\right\vert_{y=y_0}=\Im=\left.\frac{\partial v}{\partial x}\right\vert_{x=x_0}
$$

> 这又称作 **Cauchy-Riemann 方程**.

因此:

$$
\mathscr{F}'(\bm{Z}_0)=\begin{pmatrix}
    \Re & -\Im \\
    \Im & \Re
\end{pmatrix}\qquad\left(\left.\frac{\partial u}{\partial x}\right\vert_{x=x_0}=\Re=\left.\frac{\partial v}{\partial y}\right\vert_{y=y_0},\:\left.-\frac{\partial u}{\partial y}\right\vert_{y=y_0}=\Im=\left.\frac{\partial v}{\partial x}\right\vert_{x=x_0}\right)
$$

利用这个结论, 还可以得到一个更普适的计算方式:

$$
\mathscr{F}'(\bm{Z}_0)=\left.\begin{pmatrix}
    \displaystyle\frac{\partial u}{\partial x} & \displaystyle\frac{\partial u}{\partial y} \\\\
    \displaystyle\frac{\partial v}{\partial x} & \displaystyle\frac{\partial v}{\partial y}
\end{pmatrix}\right\vert_{(x,y)=(x_0,y_0)}\quad(\text{Jacobi Matrix})
$$

> 若把复变函数 $f(z)=u(x,y)+v(x,y)i$ 写作向量值函数:
> 
> $$
> f(\vec z)=f\begin{pmatrix}
>     x \\ y
> \end{pmatrix}=\begin{pmatrix}
>     u(x,y) \\ v(x,y)
> \end{pmatrix}
> $$
> 
> 则该 $\text{Jacobi Matrix}$ 正是对应的 $\bm{J} f(\vec z_0)$ 矩阵.