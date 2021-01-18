---	
layout:     post	
title:      『Probability Statistics』 Mathematical Statistics	
subtitle:   『概率统计』 数理统计    
date:       2021-01-04	   
author:     Coekjan 
catalog:    true	
katex:    true    
tags:	
    - Probability Statistics  
---

## 统计总体与样本

### 样本矩和统计量

#### 样本矩

考察总体信息时, 从总体得到的样本 $X_1,X_2,\dotsm,X_n$ 需进行一些加工, 以下是一些加工方法.

**样本均值**:

$$
\overline{X}=\frac{1}{n}\sum_{i=1}^nX_i
$$

**样本方差**:

$$
S^2=\frac{1}{n-1}\sum_{i=1}^n(X_i-\overline{X})^2
$$

**样本标准差**:

$$
S=\sqrt{\frac{1}{n-1}\sum_{i=1}^n(X_i-\overline{X})^2}
$$

**样本 $k$ 阶(原点)矩**:

$$
A_k=\frac{1}{n}\sum_{i=1}^nX_i^k
$$

**样本 $k$ 阶中心距**:

$$
B_k=\frac{1}{n}\sum_{i=1}^n(X_i-\overline{X})^k
$$

显然以上都是随机变量.

若总体的 $k$ 阶矩存在, 则样本的 $k$ 阶矩依概率收敛于总体的 $k$ 阶矩.

#### 统计量

设 $X_1,X_2,\dotsm,X_n$ 为总体的一个样本, $G(y_1,y_2,\dotsm,y_n)$ 为一个连续函数, 则称样本的函数 $G(X_1,X_2,\dotsm,X_n)$ 为一个统计量.

> 统计量中, 不能含有总体的未知参数.

##### 顺序统计量

设 $X_1,X_2,\dotsm,X_n$ 为取自总体 $X$ 的样本, $x_1,x_2,\dotsm,x_n$ 为样本的观测值. 将观测值依次序排列得到 $x_1^\star\le x_2^\star\le\dotsm\le x_n^\star$ . 规定 $X_k^\star$ 的取值为 $x_k^\star$ 就得到 $X_1,X_2,\dotsm,X_n$ 的一组顺序统计量 $X_1^\star,X_2^\star,\dotsm,X_n^\star$ , 有:

$$
X_1^\star\le X_2^\star\le\dotsm\le X_n^\star
$$

其中 $X_1^\star=\displaystyle\min_{1\le i\le n} X_i,\quad X_n^\star=\displaystyle\max_{1\le i\le n} X_i$ . 称 $D=X_n^\star-X_1^\star$ 为极差.

### 常用统计量的分布

#### 正态分布

常用结论如下

$$
\begin{aligned}
    \overline{X}&\sim N(\mu,\frac{\sigma^2}{n})\\
    \frac{\overline{X}-\mu}{\sigma/\sqrt{n}}&\sim N(0,1)\\
    \frac{X_i-\mu}{\sigma}&\sim N(0,1)\\
    \frac{1}{\sqrt{n}}\sum_{i=1}^n\frac{X_i-\mu}{\sigma}&\sim N(0,1)\\
    X_i-\overline{X}&\sim N\left(0,\frac{n-1}{n}\sigma^2\right)
\end{aligned}
$$

#### $\chi^2$ 分布

若随机变量 $X_1,X_2,\dotsm,X_n$ 相互独立, 且都服从于 $N(0,1)$ , 则称随机变量

$$
\chi^2=\sum_{i=1}^nX_i^2
$$

服从自由度为 $n$ 的 $\chi^2$ 分布, 记作 $\chi^2\sim\chi^2(n)$ .

引入 $\chi^2$ 分布的 **$\alpha$ 分位点**:

$$
P(\chi^2\le\chi_\alpha^2(n))=\alpha
$$

$\chi^2$ 分布有以下性质:
1. $\chi^2$ 分布的数学期望 $E(\chi^2(n))=n$ ;
2. $\chi^2$ 分布的方差 $D(\chi^2(n))=2n$ ;
3. 若 $X_1\sim\chi^2(n_1),X_2\sim\chi^2(n_2)$ 且两者相互独立, 则 $X_1+X_2\sim\chi^2(n_1+n_2)$

若 $X_1,X_2,\dotsm,X_n$ 相互独立, 且都服从 $N(\mu,\sigma^2)$ , 则
1. 样本均值 $\overline{X}$ 和样本方差 $S^2$ 相互独立;
2. $\displaystyle \frac{(n-1)S^2}{\sigma^2}\sim\chi^2(n-1)$ .

#### $t$ 分布

设随机变量 $X\sim N(0,1),Y\sim\chi^2(n)$ , 且 $X$ 与 $Y$ 相互独立, 则称随机变量

$$
T=\frac{X}{\sqrt{Y/n}}
$$

服从自由度为 $n$ 的 $t$ 分布, 记作 $T\sim t(n)$ .

引入 $t$ 分布的 **$\alpha$ 分位点**:

$$
P(T\le t_\alpha(n))=\alpha
$$

其性质与正态分布的分位点的性质类似.

若 $X_1,X_2,\dotsm,X_n$ 相互独立, 且都服从 $N(\mu,\sigma^2)$ , 则

$$
\frac{\overline{X}-\mu}{S/\sqrt{n}}\sim t(n-1)
$$

#### $F$ 分布

设随机变量 $X\sim \chi^2(n), Y\sim\chi^2(m)$ , 且 $X$ 与 $Y$ 相互独立, 则称随机变量

$$
F=\frac{X/n}{Y/m}
$$

服从第一自由度为 $n$ , 第二自由度为 $m$ 的 $F$ 分布, 记作 $F\sim F(n,m)$ .

引入 $F$ 分布的 **$\alpha$ 分位点**:

$$
P(F\le F_\alpha(n,m))=\alpha
$$

有性质 $F_{1-\alpha}(n,m)=\displaystyle\frac{1}{F_\alpha(n,m)}$ .

> 若 $F\sim F(n,m)$ , 则 $\displaystyle\frac{1}{F}\sim F(m,n)$ .

## 参数估计

### 点估计

#### 矩估计法

用样本的 $k$ 阶矩作为总体的 $k$ 阶矩的估计量, 建立含有待估计参数的方程, 解出待估计参数.

如:

$$
\begin{aligned}
    \frac{1}{n}\sum_{i=1}^nX_i^k&\xrightarrow{P}E(X^k)\quad&(n\rightarrow +\infty)\\
    \frac{1}{n}\sum_{i=1}^n(X_i-\overline{X})^k&\xrightarrow{P}E(X-EX)^k\quad&(n\rightarrow +\infty)
\end{aligned}
$$

#### 极大似然估计法

##### 连续型总体参数

似然函数

$$
L(\theta)=\prod_{i=1}^nf(x_i;\theta)
$$

只须求最大值点, 即可得到 $\hat{\theta}$ .

##### 离散型总体参数

似然函数

$$
L(\theta)=\prod_{i=1}^np(x_i;\theta)
$$

只须求最大值点, 即可得到 $\hat{\theta}$ .

以上两种情形中, 为求导方便, 常常使用

$$
\frac{\operatorname{d}}{\operatorname{d}\theta}\ln L(\theta)=0
$$

来求极大值点.

> 多参数情形, 则是对每一个参数求偏导.

> 不变性原理: 设 $\hat\theta$ 是 $\theta$ 的极大似然估计值, $u(\theta)$ 是 $\theta$ 的函数, 且有单值的反函数, 则 $\hat{u}=u(\hat{\theta})$ 是 $u(\theta)$ 的极大似然估计值.

#### 点估计量的优良性

##### 无偏性

设 $\hat{\theta}$ 为未知参数 $\theta$ 的估计量, 若

$$
E\left(\hat{\theta}\right)=\theta
$$

则称之为无偏.

##### 有效性

设 $\hat{\theta}_1$ 和 $\hat{\theta}_2$ 都是总体参数 $\theta$ 的**无偏估计**, 且

$$
D\left(\hat{\theta}_1\right) < D\left(\hat{\theta}_2\right)
$$

则称 $\hat{\theta}_1$ 比 $\hat{\theta}_2$ 更有效.

##### 一致性

设 $\hat{\theta}$ 是总体参量 $\theta$ 的估计量, 若对于任意 $\theta\in\varTheta$ , 当 $n\rightarrow \infty$ 时, $\hat{\theta}$ 依概率收敛于 $\theta$ , 即

$$
\forall \varepsilon>0,\lim_{n\rightarrow\infty}P\left(\left\vert\hat{\theta}-\theta\right\vert\ge\varepsilon\right)=0
$$

则称 $\hat{\theta}$ 是总体参量 $\theta$ 的一致(或相合)估计量.

> 注意使用切比雪夫不等式构造证明.

### 区间估计

设 $\theta$ 是一个待估计的参数, $\alpha(0<\alpha<1)$ 是一给定的数. 若能找到两个统计量 $\hat{\theta}_1$ 和 $\hat{\theta}_2$ , 使得

$$
P\left(\hat{\theta}_1<\theta<\hat{\theta}_1\right)=1-\alpha
$$

则称随机区间 $\left(\hat{\theta}_1,\hat{\theta}_2\right)$ 为参数 $\theta$ 的置信度为 $1-\alpha$ 的**置信区间**. 分别称 $\hat{\theta}_1$ 和 $\hat{\theta}_2$ 为置信下限和置信上限, $1-\alpha$ 称为置信水平或置信度.

> 区间长度反应精度, 置信度反应可靠程度.

一般步骤:
1. 寻找一个样本函数(**枢轴量**): $g(X_1,X_2,\dotsm,X_n,\theta)$ ;
2. 通过置信度, 确定两个常数 $a,b$ 使得
   
   $$
   P(a<g(X_1,X_2,\dotsm,X_n,\theta)<b)=1-\alpha
   $$

3. 解出 $\theta\in\left(\underline{\theta},\overline{\theta}\right)$ , 即为置信区间.

#### 正态总体均值与方差的区间估计

##### $\sigma^2$ 已知, 求 $\mu$ 的置信区间

选枢轴量 $\displaystyle \frac{\overline{X}-\mu}{\sigma/\sqrt{n}}\sim N(0,1)$

由 $\displaystyle P\left(\left\vert\frac{\overline{X}-\mu}{\sigma/\sqrt{n}}\right\vert< z_{1-\frac{\alpha}{2}}\right)=1-\alpha$ 得到 $\mu$ 的置信区间

$$
\left(\overline{X}-z_{1-\frac{\alpha}{2}}\frac{\sigma}{\sqrt{n}},\overline{X}+z_{1-\frac{\alpha}{2}}\frac{\sigma}{\sqrt{n}}\right)
$$

##### $\sigma^2$ 未知,  求 $\mu$ 的置信区间

选枢轴量 $\displaystyle \frac{\overline{X}-\mu}{S/\sqrt{n}}\sim t(n-1)$

由 $\displaystyle P\left(\left\vert\frac{\overline{X}-\mu}{S/\sqrt{n}}\right\vert<t_{1-\frac{\alpha}{2}}(n-1)\right)=1-\alpha$ 得到 $\mu$ 的置信区间

$$
\left(\overline{X}-t_{1-\frac{\alpha}{2}}(n-1)\frac{S}{\sqrt{n}},\overline{X}+t_{1-\frac{\alpha}{2}}(n-1)\frac{S}{\sqrt{n}}\right)
$$

##### $\mu$ 已知, 求 $\sigma^2$ 的置信区间

选枢轴量 $\displaystyle\sum_{i=1}^n\left(\frac{X_i-\mu}{\sigma}\right)^2\sim\chi^2(n)$

由

$$
\displaystyle P\left(\chi^2_{\frac{\alpha}{2}}(n)<\frac{\displaystyle\sum_{i=1}^n(X_i-\mu)^2}{\sigma^2}<\chi^2_{1-\frac{\alpha}{2}}(n)\right)=1-\alpha
$$

得到 $\sigma^2$ 的置信区间

$$
\left(\frac{\displaystyle\sum_{i=1}^n(X_i-\mu)^2}{\chi^2_{1-\frac{\alpha}{2}}(n)},\frac{\displaystyle\sum_{i=1}^n(X_i-\mu)^2}{\chi^2_{\frac{\alpha}{2}}(n)}\right)
$$

##### $\mu$ 未知, 求 $\sigma^2$ 的置信区间

选枢轴量 $\displaystyle\frac{(n-1)S^2}{\sigma^2}\sim\chi^2(n-1)$

由

$$
\displaystyle P\left(\chi^2_{\frac{\alpha}{2}}(n-1)<\frac{(n-1)S^2}{\sigma^2}<\chi^2_{1-\frac{\alpha}{2}}(n-1)\right)=1-\alpha
$$

得到 $\sigma^2$ 的置信区间

$$
\left(\frac{(n-1)S^2}{\chi^2_{1-\frac{\alpha}{2}}(n-1)},\frac{(n-1)S^2}{\chi^2_{\frac{\alpha}{2}}(n-1)}\right)
$$

> 若要求单侧置信区间, 则只需要取不等式的一边, 并调整分位点:
> 
> $$
> \frac{\alpha}{2}\rightarrow\alpha
> $$
> 
> 即可.

## 假设检验

在给定了显著性水平 $\alpha$ 的前提下, 接受或拒绝原假设都完全取决于样本值, 因此检验可能会发生两类错误:
* 第一类错误: 事实上原假设正确, 却拒绝
* 第二类错误: 事实上原假设错误, 却接受

> 通常把有把握, 有经验的结论作为原假设. 立场的不同会导致检验结果不同.

> 显著性水平 $\alpha$ 是指: $H_0$ 为真时, 通过检验拒绝 $H_0$ 的概率.

### 正态总体均值和方差的假设检验

#### $\sigma^2$ 已知, 检验 $\mu$

选统计量 $\displaystyle\frac{\overline{X}-\mu_0}{\sigma/\sqrt{n}}\sim N(0,1)$

##### 检验假设 $H_0:\mu=\mu_0,H_1:\mu\neq\mu_0$

根据

$$
P_{H_0}\left(\left\vert\frac{\overline{X}-\mu_0}{\sigma/\sqrt{n}}\right\vert>z_{1-\frac{\alpha}{2}}\right)=\alpha
$$

得到拒绝域:

$$
\left\vert\frac{\overline{X}-\mu_0}{\sigma/\sqrt{n}}\right\vert>z_{1-\frac{\alpha}{2}}
$$

##### 检验假设 $H_0:\mu=\mu_0,H_1:\mu>\mu_0$

> $\overline{X}$ 不能太大, 否则拒绝.

根据

$$
P_{H_0}\left(\frac{\overline{X}-\mu_0}{\sigma/\sqrt{n}}>z_{1-\alpha}\right)=\alpha
$$

得到拒绝域:

$$
\frac{\overline{X}-\mu_0}{\sigma/\sqrt{n}}>z_{1-\alpha}
$$

##### 检验假设 $H_0:\mu=\mu_0,H_1:\mu<\mu_0$

> $\overline{X}$ 不能太小, 否则拒绝.

根据

$$
P_{H_0}\left(\frac{\overline{X}-\mu_0}{\sigma/\sqrt{n}}<-z_{1-\alpha}\right)=\alpha
$$

得到拒绝域:

$$
\frac{\overline{X}-\mu_0}{\sigma/\sqrt{n}}<-z_{1-\alpha}
$$

#### $\sigma^2$ 未知, 检验 $\mu$

选统计量 $\displaystyle\frac{\overline{X}-\mu_0}{S/\sqrt{n}}\sim t(n-1)$

##### 检验假设 $H_0:\mu=\mu_0,H_1:\mu\neq\mu_0$

根据

$$
P_{H_0}\left(\left\vert\frac{\overline{X}-\mu_0}{S/\sqrt{n}}\right\vert>t_{1-\frac{\alpha}{2}}(n-1)\right)=\alpha
$$

得到拒绝域:

$$
\left\vert\frac{\overline{X}-\mu_0}{S/\sqrt{n}}\right\vert>t_{1-\frac{\alpha}{2}}(n-1)
$$

##### 检验假设 $H_0:\mu=\mu_0,H_1:\mu>\mu_0$

> $\overline{X}$ 不能太大, 否则拒绝.

根据

$$
P_{H_0}\left(\frac{\overline{X}-\mu_0}{S/\sqrt{n}}>t_{1-\alpha}(n-1)\right)=\alpha
$$

得到拒绝域:

$$
\frac{\overline{X}-\mu_0}{S/\sqrt{n}}>t_{1-\alpha}(n-1)
$$

##### 检验假设 $H_0:\mu=\mu_0,H_1:\mu<\mu_0$

> $\overline{X}$ 不能太小, 否则拒绝.

根据

$$
P_{H_0}\left(\frac{\overline{X}-\mu_0}{S/\sqrt{n}}<-t_{1-\alpha}(n-1)\right)=\alpha
$$

得到拒绝域:

$$
\frac{\overline{X}-\mu_0}{S/\sqrt{n}}<-t_{1-\alpha}(n-1)
$$

#### $\mu$ 已知, 检验 $\sigma^2$

选统计量 $\displaystyle\sum_{i=1}^n\left(\frac{X_i-\mu}{\sigma_0}\right)^2\sim \chi^2(n)$

##### 检验假设 $H_0:\sigma^2=\sigma_0^2,H_1:\sigma^2\neq\sigma_0^2$

根据

$$
P_{H_0}\left[\sum_{i=1}^n\left(\frac{X_i-\mu}{\sigma_0}\right)^2<\chi^2_{\frac{\alpha}{2}}(n)\quad\text{or}\quad\sum_{i=1}^n\left(\frac{X_i-\mu}{\sigma_0}\right)^2>\chi^2_{1-\frac{\alpha}{2}}(n)\right]=\alpha
$$

得到拒绝域:

$$
\sum_{i=1}^n\left(\frac{X_i-\mu}{\sigma_0}\right)^2<\chi^2_{\frac{\alpha}{2}}(n)\quad\text{or}\quad\sum_{i=1}^n\left(\frac{X_i-\mu}{\sigma_0}\right)^2>\chi^2_{1-\frac{\alpha}{2}}(n)
$$

##### 检验假设 $H_0:\sigma^2=\sigma_0^2,H_1:\sigma^2>\sigma_0^2$

> $\sum(X_i-\mu)^2$ 不能太大, 否则拒绝.

根据

$$
P_{H_0}\left[\sum_{i=1}^n\left(\frac{X_i-\mu}{\sigma_0}\right)^2>\chi^2_{1-\alpha}(n)\right]=\alpha
$$

得到拒绝域:

$$
\sum_{i=1}^n\left(\frac{X_i-\mu}{\sigma_0}\right)^2>\chi^2_{1-\alpha}(n)
$$

##### 检验假设 $H_0:\sigma^2=\sigma_0^2,H_1:\sigma^2<\sigma_0^2$

> $\sum(X_i-\mu)^2$ 不能太小, 否则拒绝.

根据

$$
P_{H_0}\left[\sum_{i=1}^n\left(\frac{X_i-\mu}{\sigma_0}\right)^2<\chi^2_{\alpha}(n)\right]=\alpha
$$

得到拒绝域:

$$
\sum_{i=1}^n\left(\frac{X_i-\mu}{\sigma_0}\right)^2<\chi^2_{\alpha}(n)
$$

#### $\mu$ 未知, 检验 $\sigma^2$

选统计量 $\displaystyle\frac{(n-1)S^2}{\sigma_0^2}\sim\chi^2(n-1)$

##### 检验假设 $H_0:\sigma^2=\sigma_0^2,H_1:\sigma^2\neq\sigma_0^2$

根据

$$
P_{H_0}\left(\frac{(n-1)S^2}{\sigma_0^2}<\chi^2_{\frac{\alpha}{2}}(n-1)\quad\text{or}\quad\frac{(n-1)S^2}{\sigma_0^2}>\chi^2_{1-\frac{\alpha}{2}}(n-1)\right)=\alpha
$$

得到拒绝域:

$$
\frac{(n-1)S^2}{\sigma_0^2}<\chi^2_{\frac{\alpha}{2}}(n-1)\quad\text{or}\quad\frac{(n-1)S^2}{\sigma_0^2}>\chi^2_{1-\frac{\alpha}{2}}(n-1)
$$

##### 检验假设 $H_0:\sigma^2=\sigma_0^2,H_1:\sigma^2>\sigma_0^2$

> $S^2$ 不能太大, 否则拒绝.

根据

$$
P_{H_0}\left(\frac{(n-1)S^2}{\sigma_0^2}>\chi^2_{1-\alpha}(n-1)\right)=\alpha
$$

得到拒绝域:

$$
\frac{(n-1)S^2}{\sigma_0^2}>\chi^2_{1-\alpha}(n-1)
$$

##### 检验假设 $H_0:\sigma^2=\sigma_0^2,H_1:\sigma^2<\sigma_0^2$

> $S^2$ 不能太小, 否则拒绝.

根据

$$
P_{H_0}\left(\frac{(n-1)S^2}{\sigma_0^2}<\chi^2_{\alpha}(n-1)\right)=\alpha
$$

得到拒绝域:

$$
\frac{(n-1)S^2}{\sigma_0^2}<\chi^2_{\alpha}(n-1)
$$