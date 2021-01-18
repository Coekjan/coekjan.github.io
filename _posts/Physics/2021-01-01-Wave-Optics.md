---	
layout:     post	
title:      『Physics』 Wave Optics	
subtitle:   『物理学』 波动光学    
date:       2021-01-01	   
author:     Coekjan 
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
v=\frac{1}{\sqrt{\varepsilon_0\varepsilon_r\mu_0\mu_r}}=\frac{c}{\sqrt{\varepsilon_r\mu_r}}\xlongequal{n:=\sqrt{\varepsilon_r\mu_r}}\frac{c}{n}
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

#### 迈克尔逊干涉

迈克尔需干涉仪的光路如图所示:

![]({{ '/img/WO-MichelsonInterferonmeter.svg' | prepend: site.baseurl}})

其中 $G_1$ , $G_2$ 为成 $45^\circ$ 角的平行平板, 材质与厚度完全相同. 光线 $S\rightarrow G_1\rightarrow M_2\rightarrow E$ 与 $S\rightarrow G_1\rightarrow G_2\rightarrow M_1\rightarrow G_2\rightarrow G_1\rightarrow E$ 汇聚为一束光, 在 $E$ 处可观察到两束光的干涉条纹.

其中 $M_1'$ 是虚像. 对于观察者 $E$ , 两束相干光等价于 $M_1'$ 与 $M_2$ 反射而来, 迈克尔逊干涉仪所产生的条纹就如同 $M_2$ 与 $M_1'$ 之间的空气隙产生的干涉条纹一样. 空气隙的宽度 $d$ 则可以通过可动平板 $M_2$ 的位置来调节, 从而获得不同宽度的空气隙的干涉条纹.

## 光波的衍射与惠更斯-菲涅耳原理

**波在传播过程中遇到障碍物时, 能够绕过障碍物的边缘前进.** 这种偏离直线传播的现象称为**波的衍射**.

### 菲涅尔衍射与夫琅禾费衍射

按光源, 衍射屏, 接受屏的相互间距离不同, 通常将衍射分为两类:
1. **菲涅耳衍射**: 衍射屏离光源或接受屏的距离为有限远;
2. **夫琅禾费衍射**: 衍射屏与光源和接受屏的距离都是无穷远.

实验室中可以用两个会聚透镜实现夫琅禾费衍射. 考虑到菲涅耳衍射较为复杂, 本文只讨论夫琅禾费衍射.

### 惠更斯-菲涅耳原理

菲涅尔发展了惠更斯原理吗: **波在传播过程中, 从同一波阵面上各点发出的子波, 经传播而在空间某点相遇时, 产生相干叠加**. 这个原理称为: **惠更斯-菲涅尔原理**.

可以用积分表达:

$$
U_P=\oiint_\Sigma\operatorname{d}U_P
$$

## 单缝夫琅禾费衍射

若单缝宽度为 $a$ , 在平行单色光垂直照射下, 位于单缝所处的波阵面上个点发出的子波向各个方向传播. 称衍射后某一方向传播的子波波线与平面衍射屏法线间的夹角为**衍射角**, 用 $\theta$ 表示. 衍射角相同的平行光束在会聚透镜的作用下聚焦在屏幕上. 两边缘的衍射光束之间的光程差为: $a\sin\theta$ .

![]({{ '/img/WO-1SlitDiffraction.svg' | prepend: site.baseurl}})

菲涅尔提出**半波带法**: 把单缝处的波阵面划分为整数个波带. 相邻波带之间任意相应点发出子波的光程差总是 $\displaystyle\frac{\lambda}{2}$ , 会聚后光振动完全抵消.

因此, **对于某个给定角度 $\theta$ , 单缝可以分为偶数个波带时, 出现暗纹; 单缝可以分为奇数个波带时, 出现明纹**:

$$
\left.\begin{aligned}
    \text{Dark}&&&
    a\sin\theta=\pm2k\frac{\lambda}{2}
    &&&k=1,2,3,\dotsm\\
    \text{Bright}&&&
    a\sin\theta=\pm(2k+1)\frac{\lambda}{2}
    &&&k=1,2,3,\dotsm
\end{aligned}\right\}
$$

称 $k=1$ 的两个暗纹之间的角距离为中央明纹的角宽度. 显然**半角宽度**:

$$
\begin{aligned}
    &\Delta\theta_0=\theta_1=\arcsin\frac{\lambda}{a}\\
    \xRightarrow{\theta_1\rightarrow0^+}&\Delta\theta_0\approx\frac{\lambda}{a}
\end{aligned}
$$

若知道透镜的焦距 $f$ , 且 $\theta$ 较小( $\theta<5^\circ$ ), 有: $\sin\theta\approx\tan\theta=\displaystyle\frac{x}{f}\approx \theta$ , 因此可得到暗纹离中心线的距离:

$$
x=k\frac{f\lambda}{a}\quad k=\pm1,\pm2,\dotsm
$$

### 单缝夫琅禾费衍射的实验现象

1. 条纹**对称**分布;
2. 中央明纹是其他明纹宽度的**两倍**;
3. 条纹宽与缝宽成**反比**;
4. 中央明纹光强**远大于**其他各级明纹;
5. 白光入射时, 中央为白色明纹, 两侧**内紫外红**.

### 单缝夫琅禾费衍射的光强分布

通过振动的合成, 可以计算得到:

$$
I_\varphi=I_\text{m}\left(\frac{\sin\alpha}{\alpha}\right)^2
$$

其中 $\alpha:=\displaystyle\frac{\pi a\sin\varphi}{\lambda}$ .

![]({{ '/img/WO-1SlitDiffraction-I-Alpha.svg' | prepend: site.baseurl}})

中央明纹: $I_0=I_\text{m}$

暗纹条件: $I=0\Rightarrow a\sin\varphi=\pm k\lambda(k=1,2,3,\dotsm)$ , 与半波带法结果一致.

次级明纹条件: $\displaystyle\frac{\operatorname{d}I}{\operatorname{d}\alpha}=0\Rightarrow \tan\alpha=\alpha$ , 近似解:

$$
\begin{aligned}
    \alpha&=\pm1.43\pi,\pm2.46\pi, \pm3.47\pi,\dotsm\\
    \Rightarrow a\sin\varphi&=\pm1.43\lambda,\pm2.46\lambda, \pm3.47\lambda,\dotsm
\end{aligned}
$$

可见半波带法得到的明纹位置只是一种近似.

光强分布:
* 中央明纹: $I_\text{m}$
* $1$ 级明纹: $I_1=4.72\%I_\text{m}$
* $2$ 级明纹: $I_2=1.65\%I_\text{m}$
* $3$ 级明纹: $I_3=0.80\%I_\text{m}$
* $\dotsm$

可见, 单缝衍射中, 绝大部分的能量均集中在中央明纹.

### 光学仪器的分辨本领

#### 圆孔的夫琅禾费衍射

当光波射入小圆孔时, 也会产生衍射现象. 用小圆孔替代单缝衍射中的狭缝, 也能得到衍射条纹: **中央是一个较亮的亮斑, 外围是一组同心的暗环和明环**, 这个由**第一暗环**所围成的亮斑称为**艾里斑**.

理论计算得到, 第一级暗环的衍射角 $\theta_1$ 满足:

$$
\sin\theta_1=0.61\frac{\lambda}{r}=1.22\frac{\lambda}{d}
$$

其中 $r$ 和 $d$ 是圆孔的半径和直径.

艾里斑的角半径就是第一暗环的衍射角, 若透镜焦距为 $f$ , 则艾里斑的半径为:

$$
R=f\tan\theta_1
$$

由于 $\theta_1$ 很小, 故 $\tan\theta_1\approx\sin\theta_1\approx\theta_1$ , 则:

$$
R=1.22\frac{\lambda}{d}f
$$

#### 瑞利判据

对于一个光学仪器而言, **若果一个点光源的衍射图样中央最亮处刚好与另一个点光源的衍射图样的第一个最暗处相重合, 此时两衍射图样重叠区光强度约为单个衍射图样的中央最大光强处的 $80\%$ , 一般人用眼睛刚刚能够判断出这是两个光点的像. 此时称这两个点光源恰为这一光学仪器所分辨.**

这一条件称为**瑞利判据**.

成像仪器的**最小分辨角**: $\theta_{\text{R}}=\theta_1=1.22\displaystyle\frac{\lambda}{d}$

成像仪器的**分辨本领**(或称**分辨率**): $R=\displaystyle\frac{1}{\theta_{\text{R}}}=\displaystyle\frac{d}{1.22\lambda}$

成像仪器的**最小分辨距离**: $\Delta y=f\theta_{\text{R}}=1.22\displaystyle\frac{f\lambda}{d}$

## 光栅衍射

### 光栅

称由大量等间距的平行狭缝构成的光学器件为**光栅**.

设透射光栅总缝数为 $N$ , 缝宽为 $a$ , 缝间不透光部分宽度为 $b$ , 称 $d:=a+b$ 为**光栅常数**.

### 光栅衍射条纹

**光栅衍射条纹是单缝衍射和多缝干涉的综合效果**. 干涉条纹的光强将受到单缝衍射的调制.

#### 光栅方程

产生明纹的必要条件是衍射角 $\theta$ 满足光栅方程:

$$
d\sin\theta=\pm k\lambda\quad k=0,1,2,\dotsm
$$

称式中的 $k$ 为**主极大级数**.

理论计算表明: 在相邻的两个主极大之间还存在 $N-1$ 条暗纹和 $N-2$ 条次极大, 由于次极大光强很弱且 $N$ 很大, 因此主极大之间形成一个较大的暗区.

#### 谱线缺级

由于单缝衍射的光强分布在某些衍射角 $\theta$ 时可能为 $0$ , 因此若 $\theta$ 的某些值满足光栅方程的主明纹条件, 而又满足单缝衍射的暗纹条件, 这些主明纹将消失, 称这一现象为**缺级**.

考虑 $\theta$ 满足:

$$
\left\{\begin{aligned}
    (a+b)\sin\theta&=k\lambda\\
    a\sin\theta&=k'\lambda
\end{aligned}\right.
$$

解得:

$$
k=\frac{a+b}{a}k'\quad (k'=\pm1,\pm2,\pm3,\dotsm)
$$

### 光栅光谱

复色光照射到光栅上, 除了中央明纹外, 不同波长的同一明纹的角位置不同, 并按照波长由短到长的次序由内向外依此分开排列, 每一干涉级次都有这样的一组谱线. 光栅衍射产生的这种按波长排列的谱线称为**光栅光谱**.

### 光栅的强度分布

若 $I_{10}$ 为每一条狭缝衍射时中央明纹的强度, 理论推导得到 $N$ 条缝在 $\theta$ 方向的衍射光在 $P$ 点叠加时的光强:

$$
I_\theta=I_{10}\left(\frac{\sin\alpha}{\alpha}\right)^2\left(\frac{\sin N\beta}{\sin\beta}\right)^2
$$

其中:

$$
\alpha:=\frac{\pi a}{\lambda}\sin\theta\quad\beta=\frac{\pi(a+b)}{\lambda}\sin\theta
$$

其中 $\displaystyle\left(\frac{\sin\alpha}{\alpha}\right)^2$ 为单缝衍射因子, $\displaystyle\left(\frac{\sin N\beta}{\sin\beta}\right)^2$ 为多缝干涉因子.

可以推得:
1. 明条纹光强是单独一个缝衍射产生的光强的 $N^2$ 倍;
2. 两相邻主极大之间有 $N-1$ 个极小;
3. 两相邻主极大之间有 $N-2$ 个次极大.

### 光栅的分辨本领

光栅的分辨本领是指: 把波长靠得很近的两谱线分辨清楚的本领. 用 $R$ 表示:

$$
R=\frac{\overline\lambda}{\Delta\lambda}
$$

理论推导得到, 总缝数为 $N$ 的光栅的分辨本领:

$$
R=\frac{\overline\lambda}{\Delta\lambda}=kN
$$

其中 $k$ 是欲分辨的级次, $N$ 是光栅的总缝数.

## 光波的偏振

### 自然光与偏振光

**自然光**: 光矢量在各个方向上都是对称分布的, 非偏振光.

**线偏振光**: 光矢量始终沿某一方向振动.

**圆偏振光**: 光矢量绕传播方向旋转, 旋转角速度对应于光的角频率, 光矢量端点轨迹是圆.

**椭圆偏振光**: 光矢量绕传播方向旋转, 旋转角速度对应于光的角频率, 光矢量端点轨迹是椭圆.

**部分偏振光**: 把自然光两互相垂直的分量中的一个**部分地**移去一部分.

### 起偏与检偏

称从自然光获取偏振光的过程为**起偏**, 检测入射光是否偏振的过程称为**检偏**.

### 马吕斯定律

如果入射线偏振光的振幅为 $A_1$ , 透射光振幅(不计检偏器对透射光的吸收) $A_2$ 为:

$$
A_2=A_1\cos\alpha
$$

如果入射线偏振光的光强为 $I_1$ , 透射光光强(不计检偏器对透射光的吸收) $I_2$ 为:

$$
I_2=I_1\cos^2\alpha
$$

其中, $\alpha$ 是指**偏振化方向**与入射线偏振光的夹角.

### 布儒斯特定律

自然光在两种介质的分界面上反射和折射时, 反射光和折射光都成为部分偏振光, 某些情况下, 反射光有可能会成为完全偏振光.

实验和理论同时表明: **反射光的垂直入射面振动比平行入射面振动强, 而折射光的反射光的平行入射面振动比垂直入射面振动强**.

改变入射角 $i$ 时, 反射光的偏振化程度也随之改变. 当 $i$ 等于某一特定角度时, 在反射光中只有垂直于入射面的振动, 而平行于入射面的振动完全消失, 此时反射光为完全偏振光. 这个特定的入射角称作**起偏振角**, 用 $i_\text{B}$ 表示.

实验指出, 自然光以起偏振角入射到界面时, 反射光线和入射光线相垂直: $i_\text{B}+r=90^\circ$ .

通过折射定律, 可以得到:

$$
\tan i_\text{B}=\frac{n_2}{n_1}\xlongequal{n_{21}:=n_2/n_1}n_{21}
$$

称这个规律为**布儒斯特定律**, 起偏振角也称**布儒斯特角**.

## 光波的双折射

一束光线进入方解石晶体(天然 $\rm CaCO_3$ 晶体)后分裂成两束光线, 他们沿不同方向折射, 称这种现象为**双折射**. 这是由晶体的各向异性造成的.

### 寻常光与非常光

双折射时, 总有其中一束折射光遵守折射定律, 称这束光为**寻常光**( $\rm o$ 光), 另一束为**非常光**( $\rm e$ 光).

改变入射光方向, 当光在晶体内一个特殊方向传播时不发生双折射, 这个方向称为**光轴**. 在晶体中, 我们把包含光轴和任一已知光线所组成的平面称为晶体中该光线的**主平面**.

### 惠更斯对双折射的应用

#### 单轴晶体的子波波阵面

$\rm o$ 光沿不同方向的传播速率相同, 其波面为球面; 而 $\rm e$ 光沿不同方向的传播速率不相同, 其波面是以光轴为轴的旋转椭球面. 记 $v_\text{o}$ 为 $\rm o$ 光在晶体中的传播速率, $v_\text{e}$ 为 $\rm e$ 光在晶体中沿垂直于光轴方向的传播速率.

定义 $\rm o$ 光的折射率 $n_\text{o}=\displaystyle\frac{c}{v_\text{o}}$ ; 定义 $\rm e$ 光的折射率 $n_\text{e}=\displaystyle\frac{c}{v_\text{e}}$ .

若 $v_\text{o}>v_\text{e}$ , 球面包围椭球面, 称这种晶体为**正晶体**, 如石英;

若 $v_\text{o}<v_\text{e}$ , 椭球面包围球面, 称这种晶体为**负晶体**, 如方解石.

#### 惠更斯作图法

![]({{ '/img/WO-HuygensConstruct.svg' | prepend: site.baseurl}})

### 波晶片

**波晶片**是一种由晶体制造的光学器件. 由单轴晶体上切割下来的平行薄片, 其表面与晶体的光轴平行.

假设 $\rm o$ 光与 $\rm e$ 光的光矢量为:

$$
\left\{\begin{aligned}
    E_\text{o}&=E\sin\alpha\\
    E_\text{e}&=E\cos\alpha
\end{aligned}\right.
$$

$\rm o$ 光与 $\rm e$ 光通过波晶片后的光程差为 $\delta = \vert n_\text{o}-n_\text{e}\vert d$ .

从波晶片出射的是**两束传播方向相同, 振动方向相垂直, 频率相等**的线偏振光.

一般情况下, 他们合成一束椭圆偏振光; 当 $\alpha=\displaystyle\frac{\pi}{4}$ 时, 合成光为圆偏振光.

#### 波晶片的分类

四分之一波片:

$$
\vert n_\text{o}-n_\text{e}\vert d=\frac{2k+1}{4}\lambda\quad k=0,1,2,\dotsm
$$

半波片:

$$
\vert n_\text{o}-n_\text{e}\vert d=\frac{2k+1}{2}\lambda\quad k=0,1,2,\dotsm
$$

全波片:

$$
\vert n_\text{o}-n_\text{e}\vert d=k\lambda\quad k=0,1,2,\dotsm
$$

1. 线偏振光通过四分之一波片得到椭圆或圆偏振光;
2. 线偏振光通过半波片得到转过 $2\alpha$ 的线偏振光;
3. 一定的波晶片是针对某一特定波长的光波而言的.