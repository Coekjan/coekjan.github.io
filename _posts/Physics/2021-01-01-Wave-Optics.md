---	
layout:     post	
title:      『Physics』 Wave Optics	
subtitle:   『物理学』 波动光学    
date:       2021-01-01	   
author:     Coekjan 
header-img: img/post-bg-PHY.jpg	
catalog:    true	
katex:    true    
tags:	
    - Physics  
---

## 光是电磁波

### 电磁波

凡作加速运动的电荷或电荷系都是电磁波的波源. 麦克斯韦电磁场理论表明:
1. 变化的磁场能激发涡旋电场: $\displaystyle\oint_L\vec{E}\cdot\operatorname{d}\vec{l}=-\int_S\frac{\partial \vec{B}}{\partial t}\cdot\operatorname{d}\vec{S}$
2. 传导电流和变化的电场能激发涡旋磁场: $\displaystyle\oint_L\vec{H}\cdot\operatorname{d}\vec{l}=\int_S\left(\vec{j}+\frac{\partial \vec{D}}{\partial t}\right)\cdot\operatorname{d}\vec{S}$

### 电磁波谱

波长量级 $\lambda$ ( $\rm m$ ) | $10^{4}\quad10^{2}$ | $1$ | $10^{-2}$ | $10^{-5}$ | $10^{-6}$ | $10^{-8}$ | $10^{-10}\quad10^{-12}$
:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:
电磁波种类 | 无线电波 | 微波 | 红外光 | 可见光 | 紫外光 | $\rm X$ 射线 | $\rm \gamma$ 射线

### 电磁波的波速

通过麦克斯韦电磁场方程组, 可以得到电磁波的波动方程:

$$
\nabla^2\vec{E}=\varepsilon_0\varepsilon_r\mu_0\mu_r\frac{\partial^2\vec{E}}{\partial t^2}\xlongequal{v^2:=(\varepsilon_0\varepsilon_r\mu_0\mu_r)^{-1}}\frac{1}{v^2}\frac{\partial^2\vec{E}}{\partial t^2}
$$

因此电磁波在介质中的波速为:

$$
v=\frac{1}{\sqrt{\varepsilon_0\varepsilon_r\mu_0\mu_r}}=\frac{c}{\sqrt{\varepsilon_r\mu_r}}\xlongequal{n:=\sqrt{\varepsilon_r\mu_r}}=\frac{c}{n}
$$

其中 $c=\displaystyle\frac{1}{\sqrt{\varepsilon_0\mu_0}}$ 为真空中的光速, $n$ 为介质的折射率.

### 平面简谐电磁波

电磁波动方程:

$$
\nabla^2\vec{E}=\frac{1}{v^2}\frac{\partial^2\vec{E}}{\partial t^2}
$$

电磁波波函数:

$$
\left\{\begin{aligned}
    \vec{E}_y(x,t)=\vec{E}_{0y}\cos\left[\omega\left(t-\frac{x}{v}\right)+\phi_0\right]\\
    \vec{H}_z(x,t)=\vec{H}_{0z}\cos\left[\omega\left(t-\frac{x}{v}\right)+\phi_0\right]
\end{aligned}\right.
$$

特征:
1. $\vec{E}$ 与 $\vec{H}$ 同相, 同速度传播;
2. $\vec{E}$ , $\vec{H}$ 和 $\vec{v}$ 两两垂直且满足右手螺旋定律, 电磁波是横波;
3. 关系式 $\sqrt{\varepsilon_0\varepsilon_r}E=\sqrt{\mu_0\mu_r}H$ ;
4. 波速 $v=\displaystyle\sqrt{\frac{1}{\varepsilon_0\varepsilon_r\mu_0\mu_r}}$ ;
5. 折射率 $n=\displaystyle\frac{c}{v}=\sqrt{\varepsilon_r\mu_r}$ ;
6. 电磁波平均能量密度 $w=\displaystyle\frac{1}{2}\varepsilon_0\varepsilon_rE^2+\frac{1}{2}\mu_0\mu_rH^2$ ;
7. 坡印廷矢量 $\vec{S}=\vec{E}\times\vec{H}$ ;
8. 电磁波能流密度 $I=\displaystyle\frac{1}{T}\int\vert S\vert \operatorname{d}t=\frac{1}{2}E_0H_0=\frac{1}{2}\sqrt{\frac{\varepsilon_o\varepsilon_r}{\mu_0\mu_r}}E_0^2$ .

后续研究时, 以电矢量 $\vec{E}$ 作为光矢量研究.

> 实验表明, 对人眼或仪器起作用的主要是电场.

利用折射率公式, 可以导出:

$$
\begin{aligned}
    \lambda&=\frac{\lambda_0}{n}\\
    v&=\frac{c}{n}
\end{aligned}
$$

其中, $\lambda_0$ 是光在真空中的波长, $c$ 是光在真空中的波速.

## 光波的干涉

### 波的叠加原理

**波在空间中的传播不会因为同时存在其他波而受影响**, 若干列波在重叠区某点处引起的合振动是分振动的和.

### 光的相干条件

波动光学中的干涉条件与机械波的一致: **振动频率相同, 振动方向相同和相位差恒定**. 我们在波动光学中满足这个条件的光波称为**相干光**.

#### 相干光的获取

两类经典方法:
1. **分波前法(分波阵面法)**;
2. **分振幅法**.

### 干涉条纹

#### 干涉条纹的位置

考虑以杨氏双缝干涉为例, 对屏幕上的干涉条纹位置作定量分析. 设相干光源 $S_1$ 和 $S_2$ 相距 $d$ , 中点为 $M$, 到屏幕的距离为 $D$ . 在屏幕上取一点 $P$ , $P$ 与 $S_1$ 和 $S_2$ 的距离分别为 $r_1$ 和 $r_2$ .

![]({{ '/img/WO-2SlitInterferenceCalcu.svg' | prepend: site.baseurl}})

考察从 $S_1$ 和 $S_2$ 发出的光, 到达 $P$ 处的波程差为:

$$
\delta=r_2-r_1
$$

考虑到 $D\gg d$ , $D\gg x$ , 即 $\theta$ 很小时, $\sin\theta\approx\tan\theta$ , 得:

$$
\delta=r_2-r_1\approx d\sin\theta\approx d\tan\theta=\frac{xd}{D}
$$

由波动理论可知:

若 $\delta=\displaystyle\frac{xd}{D}=\pm k\lambda$ , 则 $P$ 处为明纹, 即各级明纹离中心的距离为:

$$
x=\pm k\frac{D\lambda}{d}\quad k=0,1,2,\dotsm
$$

其中, $k=0$ 对应的明纹称为零级明纹或中央明纹. 相应于 $k=1,2,\dotsm$ 的明纹称为第一级, 第二级,...... 明纹.

若 $\delta=\displaystyle\frac{xd}{D}=\pm (2k+1)\frac{\lambda}{2}$ , 点 $P$ 处即为暗纹, 即各级暗纹离中心的距离为:

$$
x=\pm(2k+1)\frac{D\lambda}{2d}\quad k=0,1,2,\dotsm
$$

两相邻明纹或暗纹的间距都是 $\Delta x=\displaystyle\frac{D\lambda}{d}$ , 所以干涉条纹是等距离分布的.

#### 干涉条纹的强度分布

与机械波的叠加一样, 两列等强度的光波相干叠加后的强度:

$$
I=4I_0\cos^2\frac{\Delta \phi}{2}
$$

使用光强度的强弱来推导明暗条纹位置的结论与上述一致.

#### 半波损失

实验表明: **光从光疏介质射到光密介质界面反射时, 在掠入射/正入射的情况下, 反射光的相位较之入射光的相位有 $\pi$ 的突变**. 称这种现象为**半波损失**.

## 光程与光程差

### 光程

光波在介质中的路程 $x$ 相当于在真空中的路程 $nx$ ( $n$ 是介质的折射率), 称 $nx$ 为**光程**.

### 光程差

采用光程概念后, 相当于把光在不同介质中传播都折算为光在真空中的传播, 这样, 相位差 $\Delta\phi$ 可以使用光程差 $\delta$ 表达:

$$
\Delta\phi=\frac{2\pi\delta}{\lambda}
$$

#### 物象的等光程性

使用透镜只能改变光波的传播路径, **但对物象各光线不会引起附加的光程差**.

#### 半波损失与附加光程差

当且仅当发生半波损失时, 有附加光程差 $\displaystyle\frac{\lambda}{2}$ .

## 薄膜干涉

称**光波经薄膜两表面反射后相互叠加所形成的干涉现象**为**薄膜干涉**, 如肥皂膜, 水面上的油膜以及许多昆虫翅膀上的彩色花纹.

### 等倾干涉条纹

一均匀透明的平行平面介质薄膜, 其折射率为 $n$ , 厚度为 $d$ , 放在折射率为 $n_1(n_1<n)$ 的透明介质中. 波长为 $\lambda$ 的单色光入射到薄膜上表面, 入射角为 $i$ . 经薄膜的上下表面反射后, 产生一对相干的平行光束. 使用会聚透镜可以使得他们在焦平面上 $P$ 点叠加而干涉.

通过计算可以得到两束相干光的光程差为:

$$
\delta=2d\sqrt{n^2-n_1^2\sin^2i}+\frac{\lambda}{2}
$$

得到明暗条纹的条件:

$$
\left.\begin{aligned}
    \text{Bright}&&&
    \delta=2d\sqrt{n^2-n_1^2\sin^2i}+\frac{\lambda}{2}=k\lambda
    &&&k=1,2,3,\dotsm\\
    \text{Dark}&&&
    \delta=2d\sqrt{n^2-n_1^2\sin^2i}+\frac{\lambda}{2}=(2k+1)\frac{\lambda}{2}
    &&&k=0,1,2,\dotsm
\end{aligned}\right\}
$$

以上讨论的是反射光, 实质上透射光的情况也类似, 只不过没有了附加光程差:

$$
\delta=2d\sqrt{n^2-n_1^2\sin^2i}
$$

#### 增透膜与增反膜

现实中, 复杂的光学系统(如照相机)会因光反射而光能损失严重, 为使透射光加强, 可以在镜面上镀一层厚度均匀的透明薄膜, 利用薄膜干涉使得反射光减到最小, 这层膜就是**增透膜**.

考虑光从 $n=1$ 的空气向折射率为 $n_1$ 的增透膜入射, 增透膜后方是折射率为 $n_2$ 的玻璃, 折射率满足 $n<n_1<n_2$ . 若增透膜厚度为 $d$ , 光垂直入射时薄膜两表面的光程差应为 $2nd$ (半波损失在两表面均发生, 相当于没有损失), 因此相消条件:

$$
2nd=\left(k+\frac{1}{2}\right)\lambda\quad k=0,1,2,\dotsm
$$

膜的最小厚度为:

$$
d_{\text{min}}=\frac{\lambda}{4n}
$$

反射光相消, 透射光得到加强.

镀膜工艺中, 称 $nd$ 为**薄膜的光学厚度**.

有一些光学系统则需要减少透射, 以增加反射光强. 为达成这样的目的而镀上的膜为**增反膜**. 其原理与上述基本一致, 不再赘述.

### 等厚干涉条纹

如果平行光束入射到厚薄不均匀的薄膜上, 也会产生干涉现象. 称这种干涉为**等厚干涉**.

#### 劈尖干涉

两块平面玻璃片, 一端互相叠合, 另一端夹一薄纸片, 此时两玻璃片间形成的**空气薄膜**称为**空气劈尖**. 当平行单色光垂直入射这样的两个玻璃片时, 在空气劈尖上下表面引起的反射光线形成相干光. 若入射点为 $C$ , 劈尖在 $C$ 处的厚度为 $d$ , 那么在劈尖上下表面两反射光线之间的光程差为:

$$
\delta=2d+\frac{\lambda}{2}
$$

得到明暗条纹的条件:

$$
\left.\begin{aligned}
    \text{Bright}&&&
    \delta=2d+\frac{\lambda}{2}=k\lambda
    &&&k=1,2,3,\dotsm\\
    \text{Dark}&&&
    \delta=2d+\frac{\lambda}{2}=(2k+1)\frac{\lambda}{2}
    &&&k=0,1,2,\dotsm
\end{aligned}\right\}
$$

干涉条纹为平行于劈尖棱边的直线条纹. 每一明暗条纹都与一定的 $k$ 值相对应, 也就与劈尖的厚度相当. 所以, 可以确定任意两个相邻明纹或任意两个相邻暗纹之间的距离:

$$
l\sin\theta=d_{k+1}-d_k=\frac{\lambda}{2}
$$

可见, 若已知光的波长, 可以通过测定相邻干涉明纹间的距离来确定劈尖的夹角 $\theta$ .

同时可见, 若夹角相当大, 干涉条纹就紧密得无法分开, 因此劈尖干涉条纹只能在很尖的劈尖上看到.

#### 牛顿环干涉

在一块平整的光学玻璃片 $B$ 上, 放一曲率半径 $R$ 很大的平凸透镜 $A$ . $A$ 与 $B$ 之间形成了一劈尖形空气薄膜. 当平行光束垂直设想平凸透镜, 可以看到透镜表面出现一组干涉条纹, 这些干涉条纹是以接触点 $O$ 为中心的同心圆环, 称为**牛顿环**.

与劈尖膜的干涉类似, 牛顿环干涉的明暗条纹条件:

$$
\left.\begin{aligned}
    \text{Bright}&&&
    \delta=2d+\frac{\lambda}{2}=k\lambda
    &&&k=1,2,3,\dotsm\\
    \text{Dark}&&&
    \delta=2d+\frac{\lambda}{2}=(2k+1)\frac{\lambda}{2}
    &&&k=0,1,2,\dotsm
\end{aligned}\right\}
$$

由几何关系, 容易得到: $r^2=R^2-(R-d)^2=2Rd-d^2$ , 由于 $R\gg d$ , 因此可以略去 $d^2$ 项, 于是:

$$
d=\frac{r^2}{2R}
$$

于是得到条纹半径 $r$ 与 $k$ , $R$ 的关系:

$$
\left.\begin{aligned}
    \text{Bright}&&&
    r^2=\frac{2k-1}{2}R\lambda
    &&&k=1,2,3,\dotsm\\
    \text{Dark}&&&
    r^2=kR\lambda
    &&&k=0,1,2,\dotsm
\end{aligned}\right\}
$$

通过测定条纹半径, 即可算得曲率半径.

## 光波的衍射与惠更斯-菲涅耳原理

**波在传播过程中遇到障碍物时, 能够绕过障碍物的边缘前进.** 这种偏离直线传播的现象称为**波的衍射**.