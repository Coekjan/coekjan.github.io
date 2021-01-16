---	
layout:     post	
title:      『Computer Architecture』 Single Cycle CPU	
subtitle:   『计算机体系结构』 单周期CPU    
date:       2021-01-17	   
author:     Coekjan 
header-img: img/post-bg-CA.jpg	
catalog:    true    
katex:  true    
tags:	
    - Computer Architecture  
---

## MIPS指令特征

### 三大类指令

MIPS指令均为32位的01串, 根据这32位的功能划分, 可以粗略得到三大指令类型: R型, I型, J型.

#### R型

R型指令的编码如下:

$31\qquad26$ | $25\qquad21$ | $20\qquad16$ | $15\qquad11$ | $10\qquad6$| $5\qquad0$
:-:|:-:|:-:|:-:|:-:|:-:
**opcode**|**rs**|**rt**|**rd**|**shamt**|**funct**
$6$ | $5$ | $5$ | $5$ | $5$ | $6$

> R型的opcode为`000000`; 使用不同的funct用以区分不同的R型指令.

#### I型

$31\qquad26$ | $25\qquad21$ | $20\qquad16$ | $15\qquad\qquad\qquad\qquad\qquad\qquad\qquad0$ 
:-:|:-:|:-:|:-:
**opcode**|**rs**|**rt**|**imm16**
$6$ | $5$ | $5$ | $16$

#### J型

$31\qquad26$ | $25\qquad\qquad\qquad\qquad\qquad\qquad\qquad\qquad\qquad\qquad\qquad\qquad0$
:-:|:-:|:-:|:-:
**opcode**|**imm26**
$6$ | $26$

### 指令RTL

> **RTL**: Register Transfer Language

在MIPS指令集中, 会使用RTL给出指令的功能. 理解RTL是实现指令的关键.

下面以指令集{`addu`, `subu`, `addiu`, `lui`, `ori`, `lw`, `sw`, `beq`, `j`}的RTL为例, 作一些解释.

指令| 类型 | RTL | 解释
:-:|:-:|:-:|:--
`addu`| R | $$\begin{aligned}&\rm GRF[rd]\leftarrow\rm GRF[rs]+GRF[rt]\\& \rm PC\leftarrow\rm PC+4\end{aligned}$$ | 将rs号寄存器的值, 与rt号寄存器的值**相加**, 结果**存入rd号**寄存器.
`subu` | R | $$\begin{aligned}&\rm GRF[rd]\leftarrow\rm GRF[rs]-GRF[rt]\\& \rm PC\leftarrow\rm PC+4\end{aligned}$$ | 将rs号寄存器的值, 与rt号寄存器的值**相减**, 结果**存入rd号**寄存器.
`addiu` | I | $$\begin{aligned}&\rm GRF[rt]\leftarrow\rm GRF[rs]+sign\_ext(imm16)\\ &\rm PC\leftarrow\rm PC+4\end{aligned}$$ | 将rs号寄存器值, 与imm16**符号扩展**后的值**相加**, 结果**存入rt号**寄存器.
`lui` | I | $$\begin{aligned}&\rm GRF[rt]\leftarrow\rm imm16\:\vert\vert\:0^{16}\\ &\rm PC\leftarrow\rm PC+4\end{aligned}$$ | 将imm16**高位加载**, **存入rt号**寄存器.
`ori` | I | $$\begin{aligned}&\rm GRF[rt]\leftarrow\rm GRF[rs]\:\operatorname{or}\:zero\_ext(imm16)\\& \rm PC\leftarrow\rm PC+4\end{aligned}$$ | 将rs号寄存器的值, 与imm16**零扩展**后的值**按位或**, 结果**存入rt号**寄存器.
`lw` | I | $$\begin{aligned}&\rm GRF[rt]\leftarrow\rm DM[GRF [rs]+sign\_ext(imm16) ]\\& \rm PC\leftarrow\rm PC+4\end{aligned}$$ | 将rs号寄存器的值, 与imm16**符号扩展**的值**相加**, 以之为地址访问内存中的字, 将整字**存入rt号**寄存器.
`sw` | I | $$\begin{aligned}&\rm DM[GRF [rs]+sign\_ext(imm16) ]\leftarrow\rm GRF[rt]\\& \rm PC\leftarrow\rm PC+4\end{aligned}$$ | 将rs号寄存器的值, 与imm16**符号扩展**的值**相加**, 以之为地址定位到内存字单元, 将rt号寄存器的值整字存入其中.
`beq` | I | $$\begin{aligned}&\rm if\quad GRF[rs]==GRF[rt]\quad then\\&\qquad\rm PC\leftarrow PC+4+sign\_ext(imm16\:\vert\vert\:0^2)\\& \rm else\\&\rm\qquad PC\leftarrow PC+4\end{aligned}$$ | 若rs号寄存器的值, 与rt号寄存器的值**相等**, 则将imm16**低位补两位0**, 并**符号扩展**后得到偏移量, PC相对于下一条指令作**偏移**.
`j` | J | $$\rm PC\leftarrow PC[31\dots28]\:\vert\vert\:imm26\:\vert\vert\:0^2$$ | 将imm26**低位补两位0**, 并**高位拼接PC[31:28]**, 得到新PC值, **存入PC**寄存器.

> 我们需要找到指令的共性, 从而得知CPU需要具备的关键部件.

## CPU关键部件

从上述对MIPS指令的特征分析, 我们指出, CPU中必须包含如下的关键部件.

### PC (Program Counter)

程序计数器: 引导指令存储器读出指令.

![]({{ '/img/CPU-PC.svg' | prepend: site.baseurl}})

### IM (Instruction Memory)

指令存储器: 存储指令的只读模块, 输出的指令由地址端支配.

### GRF (General Register File)

### EXT (Extender)

### ALU(Arithmetic Logic Unit)

### DM (Data Memory)