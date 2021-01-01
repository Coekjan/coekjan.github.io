---	
layout:     post	
title:      『Physics』 Mechanical Vibration	
subtitle:   『物理学』 机械振动    
date:       2020-12-30	   
author:     Coekjan 
header-img: img/post-bg-PHY.jpg	
catalog:    true	
katex:    true    
tags:	
    - Physics  
---

## 谐振动

### 谐振动的特征与表达式

考虑一个质量为 $m$ 的物体系于一端固定的轻弹簧(弹簧的质量相对于物体质量而言可以忽略不计)的自由端, 弹簧和物体组成的系统称作**弹簧振子**. 物体所受合外力为零的位置称作**平衡位置**.

若把物体在弹簧的方向上稍加移动, 偏移平衡位置后释放, 则物体会在平衡位置附近作往复运动. 下面研究这样的往复运动有何特征.

取物体的平衡位置为坐标原点 $O$ , 取物体运动轨迹所在直线为 $Ox$ 轴, 向右为正方向. 按胡克定律, 在弹簧的弹性限度内, 物体所受的拉力 $F$ 与物体相对于平衡位置的位移 $x$ 成正比关系:

$$
F=-kx
$$

比例系数 $k$ 称作弹簧的劲度系数. 根据牛顿第二定律, 物体的加速度:

$$
\begin{aligned}
    &\frac{\operatorname{d}^2x}{\operatorname{d}t^2}=-\frac{k}{m}x\xlongequal{\omega^2:=k/m}-\omega^2x\\
    \Leftrightarrow&\frac{\operatorname{d}^2x}{\operatorname{d}t^2}+\omega^2x=0
\end{aligned}
$$

求解此微分方程, 可得:

$$
x=A\cos(\omega t+\phi_0)
$$

其中 $A$ 与 $\phi_0$ 均为积分常量. 我们称满足这样关系的运动称作**谐振动**.

有时我们会使用其复指数形式:

$$
x=Ae^{i(\omega t+\phi_0)}
$$

通过谐振动的 $x-t$ 方程, 还可得到 $v-t$ 和 $a-t$ 方程:

$$
v=-v_m\sin(\omega t+\phi_0)
$$

$$
a=-a_m\cos(\omega t+\phi_0)
$$

其中 $v_m=\omega A$ , $a_m=\omega^2A$ 称作速度峰值, 加速度峰值.

若给定初始条件:

$$
\left\{\begin{aligned}
    x_0&=A\cos\phi_0\\
    v_0&=-\omega A\sin\phi_0
\end{aligned}\right.
$$

可以得到积分变量的值:

$$
\left\{\begin{aligned}
    A&=\sqrt{x_0^2+\frac{v_0^2}{\omega^2}}\\
    \phi_0&=\arctan\left(-\frac{v_0}{\omega x_0}\right)
\end{aligned}\right.
$$

### 谐振动的特征量

1. 振幅 $A$ : 作谐振动的物体离开平衡位置的最大位移的绝对值.
2. 周期 $T$ : 完成一次完整振动所经历的时间.
3. 频率 $\nu$ : 单位时间内物体所作的完全振动的次数, 也称圆频率.
4. 相位 $\phi$ : 振动物体在某一时刻 $t$ 时具有相位 $\omega t +\phi_0$ ; $t=0$ 时具有的相位称为**初相**.

易见, 角速度 $\omega$ , 周期 $T$ , 频率 $\nu$ 间有以下关系:

$$
\begin{aligned}
    T&=\frac{2\pi}{\omega}\\
    \nu&=\frac{1}{T}\\
    \omega&=2\pi\nu
\end{aligned}
$$

### 常见的谐振动

#### 单摆

一根长度为 $l$ 的不伸缩细线, 上端固定(或一根刚性轻杆, 上端与无摩擦的铰链相连), 下端悬一小重物 $m$ , 把重物略加移动后就可以在竖直平面内来回摆动, 称这种装置为**单摆**.

当摆线与竖直方向成 $\theta$ 角时, 重物受到重力 $G$ 与拉力 $F$ 两个不共线的力作用. 重力的切向分量为 $mg\sin\theta$ , 此即为重物受到的合外力.

考虑角位移 $\theta$ 从竖直位置算起, 并规定逆时针方向为正, 则重物有角加速度: $\beta=\displaystyle\frac{\operatorname{d}^2\theta}{\operatorname{d}t^2}$ , 因此重物的切向加速度为: $a_t=l\displaystyle\frac{\operatorname{d}^2\theta}{\operatorname{d}t^2}$ , 由牛顿第二定律:

$$
-mg\sin\theta=ml\frac{\operatorname{d}^2\theta}{\operatorname{d}t^2}
$$

当 $\theta$ 很小时, $\sin\theta\approx\theta$ , 因此:

$$
\frac{\operatorname{d}^2\theta}{\operatorname{d}t^2}=-\frac{g}{l}\theta\xlongequal{\omega^2:=g/l}-\omega^2\theta
$$

可见这是谐振动, 振动表达式为:

$$
\theta=\theta_m\cos(\omega t + \phi_0)
$$

振动周期为:

$$
T=\frac{2\pi}{\omega}=2\pi\sqrt{\frac{l}{g}}
$$

理论上, $\theta_m=15^\circ$ 时, 真实周期与上述计算结果相差不超过 $0.5\%$

#### 复摆

称一个可绕定轴 $O$ 摆动的刚体为**复摆**, 也称**物理摆**.

平衡时, 摆的重心 $C$ 在距离轴的正下方 $h$ 处; 摆动时, 重心与轴的连线 $OC$ 偏离平衡位置. 设某一时刻 $t$ , $OC$ 与竖直位置夹角为 $\theta$ , 规定逆时针方向的角位移为正.

此时复摆受到对于轴 $O$ 的力矩为:

$$
M=-mgh\sin\theta
$$

刚体绕 $O$ 的转动惯量若为 $J$ , 根据转动定律:

$$
J\frac{\operatorname{d}^2\theta}{\operatorname{d}t^2}=-mgh\sin\theta
$$

当 $\theta$ 很小时, $\sin\theta\approx\theta$ , 因此:

$$
\frac{\operatorname{d}^2\theta}{\operatorname{d}t^2}=-\frac{mgh}{J}\theta\xlongequal{\omega^2:=mgh/J}-\omega^2\theta
$$

可见这是谐振动, 振动表达式为:

$$
\theta=\theta_m\cos(\omega t + \phi_0)
$$

振动周期为:

$$
T=\frac{2\pi}{\omega}=2\pi\sqrt{\frac{J}{mgh}}
$$

### 谐振动的能量

现以弹簧振子为例研究谐振动的能量.

谐振动的动能:

$$
E_k=\frac{1}{2}mv^2=\frac{1}{2}m\omega^2A^2\sin^2(\omega t+\phi_0)
$$

谐振动的势能:

$$
E_p=\frac{1}{2}kx^2=\frac{1}{2}kA^2\cos^2(\omega t+\phi_0)
$$

考虑到 $\omega^2=\displaystyle\frac{k}{m}$ , 则总能量:

$$
E=E_k+E_p=\frac{1}{2}kA^2
$$

计算式 $E=\displaystyle\frac{1}{2}kA^2$ 适用于任意谐振动.

另外, 根据三角函数的半角公式, 不难证明**动能, 势能的变化频率是振子频率的两倍**.

## 谐振动的合成

### 一维谐振动的合成

#### 同一直线上两个同频率的谐振动合成

考虑两个谐振动:

$$
\left\{\begin{aligned}
    x_1&=A_1\cos(\omega t+\phi_{01})\\
    x_2&=A_2\cos(\omega t+\phi_{02})
\end{aligned}\right.
$$

一个质点若同时参与这两个谐振动, 则有合位移:

$$
\begin{aligned}
    x=&x_1+x_2=A_1\cos(\omega t+\phi_{01})+A_2\cos(\omega t+\phi_{02})\\
    =&A_1\cos\phi_{01}\cos\omega t-A_1\sin\phi_{01}\sin\omega t+A_2\cos\phi_{02}\cos\omega t-A_2\sin\phi_{02}\sin\omega t\\
    =&(A_1\cos\phi_{01}+A_2\cos\phi_{02})\cos\omega t-(A_1\sin\phi_{01}+A_2\sin\phi_{02})\sin\omega t
\end{aligned}
$$

通过三角函数的辅助角公式, 得到化简结果为:

$$
x=A\cos(\omega t+\phi_0)
$$

其中:

$$
A=\sqrt{A_1^2+A_2^2+2A_1A_2\cos(\phi_{01}-\phi_{02})}
$$

$$
\phi_0=\arctan\frac{A_1\sin\phi_{01}+A_2\sin\phi_{02}}{A_1\cos\phi_{01}+A_2\cos\phi_{02}}
$$

可以轻松得到一些结论:

1. 若两振动同相 $\phi_{01}=\phi_{02}+2k\pi\quad (k=0,\pm1,\pm2,\dotsm)$ , 则合振动的振幅为 $A=A_1+A_2$ ;
2. 若两振动反相 $\phi_{01}=\phi_{02}+(2k+1)\pi\quad (k=0,\pm1,\pm2,\dotsm)$ , 则合振动的振幅为 $A=\vert A_1-A_2\vert$
3. 若两振动既不同相也不反相, 则合振动的振幅在区间 $(\vert A_1-A_2\vert,A_1+A_2)$ 内.

##### 同一直线上的 $N$ 个同频率同振幅谐振动合成

若有一系列谐振动:

$$
\left\{\begin{aligned}
    x_1&=a\cos\omega t\\
    x_2&=a\cos(\omega t+\varepsilon)\\
    x_3&=a\cos(\omega t+2\varepsilon)\\
    &\vdots\\
    x_N&=a\cos\left[\omega t+(N-1)\varepsilon\right]
\end{aligned}\right.
$$

此时, 考虑使用旋转矢量法计算:

![]({{ '/img/SHM-VectorRotation.svg' | prepend: site.baseurl}})

合振动方程形式应为:

$$
x=A\cos(\omega t + \phi)
$$

可以通过上图解得:

$$
\left\{\begin{aligned}
    A&=a\frac{\displaystyle\sin\frac{N\varepsilon}{2}}{\displaystyle\sin\frac{\varepsilon}{2}}\\
    \phi&=\frac{N-1}{2}\varepsilon
\end{aligned}\right.
$$

因此合振动方程表达式:

$$
x=a\frac{\displaystyle\sin\frac{N\varepsilon}{2}}{\displaystyle\sin\frac{\varepsilon}{2}}\cos\left(\omega t+\frac{N-1}{2}\varepsilon\right)
$$

#### 同一直线上两个不同频率的谐振动合成

考虑两个谐振动:

$$
\left\{\begin{aligned}
    x_1&=A_1\cos(\omega_1t+\phi_{01})\\
    x_2&=A_2\cos(\omega_2t+\phi_{02})
\end{aligned}\right.
$$

合振动: $x=x_1+x_2=A_1\cos(\omega_1t+\phi_{01})+A_2\cos(\omega_2t+\phi_{02})$ 一般情况下是相当复杂的. 现在我们讨论两个频率接近且 $\vert \omega_1-\omega_2\vert\ll\min(\omega_1, \omega_2)$ 这样的情况, 为进一步简化, 我们令 $A_1=A_2=A$ , $\phi_{01}=\phi_{02}=\phi_0$ , 上式化为:

$$
x=2A\cos\left(\frac{\omega_1-\omega_2}{2}t\right)\cos\left(\frac{\omega_1+\omega_2}{2}t+\phi_0\right)
$$

因为 $\vert \omega_1-\omega_2\vert\ll\min(\omega_1, \omega_2)$ , 式中第一项 $\displaystyle\cos\left(\frac{\omega_1-\omega_2}{2}t\right)$ 随时间缓慢变化, 第二项则近似于角频率为 $\omega_1$ 或 $\omega_2$ 的简谐函数, 故合振动可以看作角频率 $\displaystyle\frac{\omega_1+\omega_2}{2}\approx\omega_1\approx\omega_2$ , 振幅 $\left\vert2A\cos\displaystyle\frac{\omega_1-\omega_2}{2}t\right\vert$ 的谐振动.

这种合振动出现时强时弱周期性缓慢变化的现象称作**拍**. 振幅变化的周期 $\tau$ 由 $\left\vert\displaystyle\frac{\omega_1-\omega_2}{2}\right\vert\tau=\pi$ 决定, 振幅变化的频率 $\nu_{\text{beat}}$ 称作拍频:

$$
\nu_{\text{beat}}=\frac{1}{\tau}=\left\vert\frac{\omega_1-\omega_2}{2\pi}\right\vert=\vert\nu_2-\nu_1\vert
$$

示例如下:

![]({{ '/img/SHM-Beat1.svg' | prepend: site.baseurl}})

$$
\LARGE{+}
$$

![]({{ '/img/SHM-Beat2.svg' | prepend: site.baseurl}})

$$
\LARGE{\Downarrow}
$$

![]({{ '/img/SHM-BeatComb.svg' | prepend: site.baseurl}})

### 二维谐振动的合成

#### 两个相互垂直的同频率谐振动合成

考虑两个谐振动:

$$
\left\{\begin{aligned}
    x&=A_1\cos(\omega t+\phi_{01})\\
    y&=A_2\cos(\omega t+\phi_{02})
\end{aligned}\right.
$$

把参量 $t$ 消去, 得到轨迹方程:

$$
\frac{x^2}{A_1^2}+\frac{y^2}{A_2^2}-2\frac{xy}{A_1A_2}\cos(\phi_{01}-\phi_{02})=\sin^2(\phi_{01}-\phi_{02})
$$

若两振动同相 $\phi_{01}=\phi_{02}+k\pi\quad (k=0,\pm1,\pm2,\dotsm)$ . 上式化为:

$$
\frac{x}{A_1}\pm\frac{y}{A_2}=0
$$

是一条直线.

若两振动不同相, 则上式为椭圆方程.

#### 两个相互垂直的不同频率谐振动合成

若两个振动频率相差很大, 且有简单整数比, 可以得到稳定的封闭图线, 称这些图线为**李萨如图形**. 若李萨如图形在横轴上有 $n_x$ 个交点, 在纵轴上有 $n_y$ 个交点, 则有:

$$
\frac{n_x}{n_y}=\frac{\nu_y}{\nu_x}
$$

注意, 选取轴线时要使得交点数**极大**.

## 阻尼振动与受迫振动

### 阻尼振动

对于一个存在阻尼的振动系统, 当速度不太大时, 可以认为阻尼力与速度大小成正比:

$$
F_f=-\gamma\frac{\operatorname{d}x}{\operatorname{d}t}
$$

其中 $\gamma$ 称作阻尼系数.

设质量为 $m$ 的物体, 在弹性力(或准弹性力)和阻尼力的共同作用下运动, 则物体的运动方程为:

$$
m\frac{\operatorname{d}^2x}{\operatorname{d}t^2}=-kx-\gamma\frac{\operatorname{d}x}{\operatorname{d}t}
$$

命 $\displaystyle\frac{k}{m}=\omega_0^2$ , $\displaystyle\frac{\gamma}{m}=2\delta$ , 这里 $\omega_0$ 称作无阻尼时振子的**固有角频率**, $\delta$ 称作**阻尼系数**. 上式改写为:

$$
\frac{\operatorname{d}^2x}{\operatorname{d}t^2}+2\delta\frac{\operatorname{d}x}{\operatorname{d}t}+\omega_0^2x=0
$$

在 $\delta < \omega_0$ 时, 即阻尼较小时, 此微分方程的解为:

$$
x=A_0e^{-\delta t}\cos(\omega't+\phi_0')
$$

式中:

$$
\omega'=\sqrt{\omega_0^2-\delta^2}=\sqrt{\frac{k}{m}-\frac{\gamma^2}{4m^2}}
$$

另外, $A_0$ 和 $\phi_0'$ 是积分常量, 由初始条件决定.

阻尼振动的周期:

$$
T'=\frac{2\pi}{\omega'}=\frac{2\pi}{\sqrt{\omega_0^2-\delta^2}}<\frac{2\pi}{\omega_0}=T_0
$$

也就是说, 由于阻尼, 振动变慢了.

**小阻尼**: 若 $\delta < \omega_0$ , 那么物体相当于作振动幅度逐渐减小的谐振动.

**临界阻尼**: 若 $\delta = \omega_0$ , 那么微分方程的解为: $x=(C_1x+C_2)e^{-\delta t}$, 物体将**单侧**平滑地回到平衡位置.

**过阻尼**: 若 $\delta > \omega_0$ , 那么物体将**单侧**缓慢地回到平衡位置. 其向平衡位置移动的速度将慢于临界阻尼情形.

### 受迫振动

物体在周期性外力作用下发生的振动为**受迫振动**. 这种周期性的外力称作**驱动力**.

假设驱动力 $F=F_0\cos\omega_dt$ , 其中 $F_0$ 是驱动力的幅值, $\omega_d$ 称作驱动力的角频率. 物体在弹性力, 阻尼力, 驱动力的作用下运动的方程为:

$$
m\frac{\operatorname{d}^2x}{\operatorname{d}t^2}=-kx-\gamma\frac{\operatorname{d}x}{\operatorname{d}t}+F_0\cos\omega_dt
$$

令 $\displaystyle\frac{k}{m}=\omega_0^2$ , $\displaystyle\frac{\gamma}{m}=2\delta$ , 则上式可写作:

$$
\frac{\operatorname{d}^2x}{\operatorname{d}t^2}+2\delta\frac{\operatorname{d}x}{\operatorname{d}t}+\omega_0^2x=\frac{F_0}{m}\cos\omega_dt
$$

阻尼较小时, 上式的解为:

$$
x=A_0e^{-\delta t}\cos(\sqrt{\omega_0^2-\delta^2}t+\phi_0')+A\cos(\omega_dt+\phi)
$$

此式表示受迫振动一开始时是由一个减幅振动和一个等幅振动合成的, 比较复杂. 而经过一段时间后, 第一项可以忽略不计, 振动达到稳态:

$$
x=A\cos(\omega_dt+\phi)
$$

理论计算得到:

$$
\left\{\begin{aligned}
    A&=\frac{F_0}{m\sqrt{(\omega_0^2-\omega_d^2)^2+4\delta^2\omega_d^2}}\\
    \phi&=\arctan\left(-\frac{2\delta\omega_d}{\omega_0^2-\omega_d^2}\right)
\end{aligned}\right.
$$

稳态时, 物体速度:

$$
v=v_m\cos\left(\omega_dt+\phi+\frac{\pi}{2}\right)
$$

其中:

$$
v_m=\frac{\omega_d F_0}{m\sqrt{(\omega_0^2-\omega_d^2)^2+4\delta^2\omega_d^2}}
$$

从上述分析结果来看, 受迫振动有以下特征:
1. 角频率是驱动力的角频率, 而不是振子的固有角频率;
2. 受迫振动的振幅依赖于振子的性质, 阻尼的大小和驱动力的特征, 而不是决定于振子的初始状态.

#### 共振

##### 位移振动

振幅达到最大的现象称为**位移共振**. 即令 $\displaystyle\frac{\operatorname{d}A}{\operatorname{d}\omega_d}=0$ , 得到共振角频率:

$$
\omega_r=\sqrt{\omega_0^2-2\delta^2}
$$

可见位移共振时, 驱动力的角频率略小于系统的固有角频率. 阻尼越小, 共振角频率越接近于固有角频率, 共振振幅越大.

##### 速度共振

速度达到最大的现象称为**速度共振**. 即令 $\displaystyle\frac{\operatorname{d}v_m}{\operatorname{d}\omega_d}=0$ , 得到共振角频率:

$$
\omega_r=\omega_0
$$

可见"频率相等时共振"的说法是指速度共振, 但是阻尼很小时, 速度共振和位移共振可以不加区分.