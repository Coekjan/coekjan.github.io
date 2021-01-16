---	
layout:     post	
title:      『Computer Architecture』 Single Cycle CPU	
subtitle:   『计算机体系结构』 单周期CPU    
date:       2021-01-17	   
author:     Coekjan 
header-img: img/post-bg-CA.jpg	
catalog:    true	
tags:	
    - Computer Architecture  
---

## MIPS指令特征

### 三大类指令

MIPS指令均为32位的01串, 根据这32位的功能划分, 可以粗略得到三大指令类型: R型, I型, J型.

#### R型

R型指令的编码如下:

**opcode**|**rs**|**rt**|**rd**|**shamt**|**funct**
:-:|:-:|:-:|:-:|:-:|:-:
6 | 5 | 5 | 5 | 5 | 6

> R型的opcode为`000000`; 使用不同的funct用以区分不同的R型指令.

#### I型

**opcode**|**rs**|**rt**|**imm16**
:-:|:-:|:-:|:-:
6 | 5 | 5 | 16

#### J型

**opcode**|**imm26**
:-:|:-:|:-:|:-:
6 | 26

### 指令RTL

> **RTL**: Register Transfer Language

在MIPS指令集中, 会使用RTL给出指令的功能. 理解RTL是实现指令的关键.

下面以指令集{`addu`, `subu`, `addiu`, `lui`, `ori`, `lw`, `sw`, `beq`, `j`}的RTL为例, 作一些解释.

* `addu`
R型, RTL如下

$$
\begin{aligned}&\rm GRF[rd]\leftarrow\rm GRF[rs]+GRF[rt]\\& \rm PC\leftarrow\rm PC+4\end{aligned}
$$

> **解释**: 将rs号寄存器的值, 与rt号寄存器的值**相加**, 结果**存入rd号**寄存器.


* `subu`
R型, RTL如下

$$
\begin{aligned}&\rm GRF[rd]\leftarrow\rm GRF[rs]-GRF[rt]\\& \rm PC\leftarrow\rm PC+4\end{aligned}
$$

> **解释**: 将rs号寄存器的值, 与rt号寄存器的值**相减**, 结果**存入rd号**寄存器.


* `addiu`
I型, RTL如下

$$
\begin{aligned}&\rm GRF[rt]\leftarrow\rm GRF[rs]+sign\_ext(imm16)\\ &\rm PC\leftarrow\rm PC+4\end{aligned}
$$

> **解释**: 将rs号寄存器值, 与imm16**符号扩展**后的值**相加**, 结果**存入rt号**寄存器.


* `lui`
I型, RTL如下

$$
\begin{aligned}&\rm GRF[rt]\leftarrow\rm imm16\:\vert\vert\:0^{16}\\ &\rm PC\leftarrow\rm PC+4\end{aligned}
$$

> **解释**: 将imm16**高位加载**, **存入rt号**寄存器.


* `ori`
I型, RTL如下

$$
\begin{aligned}&\rm GRF[rt]\leftarrow\rm GRF[rs]\:\operatorname{or}\:zero\_ext(imm16)\\& \rm PC\leftarrow\rm PC+4\end{aligned}
$$

> **解释**: 将rs号寄存器的值, 与imm16**零扩展**后的值**按位或**, 结果**存入rt号**寄存器.


* `lw`
I型, RTL如下

$$
\begin{aligned}&\rm GRF[rt]\leftarrow\rm DM[GRF [rs]+sign\_ext(imm16) ]\\& \rm PC\leftarrow\rm PC+4\end{aligned}
$$

> **解释**: 将rs号寄存器的值, 与imm16**符号扩展**的值**相加**, 以之为地址访问内存中的字, 将整字**存入rt号**寄存器.


* `sw`
I型, RTL如下

$$
\begin{aligned}&\rm DM[GRF [rs]+sign\_ext(imm16) ]\leftarrow\rm GRF[rt]\\& \rm PC\leftarrow\rm PC+4\end{aligned}
$$

> **解释**: 将rs号寄存器的值, 与imm16**符号扩展**的值**相加**, 以之为地址定位到内存字单元, 将rt号寄存器的值整字存入其中.


* `beq`
I型, RTL如下

$$
\begin{aligned}&\rm if\quad GRF[rs]==GRF[rt]\quad then\\&\qquad\rm PC\leftarrow PC+4+sign\_ext(imm16\:\vert\vert\:0^2)\\& \rm else\\&\rm\qquad PC\leftarrow PC+4\end{aligned}
$$

> **解释**: 若rs号寄存器的值, 与rt号寄存器的值**相等**, 则将imm16**低位补两位0**, 并**符号扩展**后得到偏移量, PC相对于下一条指令作**偏移**.


* `j`
J型, RTL如下

$$
\rm PC\leftarrow PC[31\dots28]\:\vert\vert\:imm26\:\vert\vert\:0^2
$$

> **解释**: 将imm26**低位补两位0**, 并**高位拼接PC[31:28]**, 得到新PC值, **存入PC**寄存器.



我们需要找到指令的共性, 从而得知CPU需要具备的关键部件. 上述指令中共性部分已加粗表示.

// 待续...