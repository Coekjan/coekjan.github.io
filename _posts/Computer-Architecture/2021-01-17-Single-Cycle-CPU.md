---	
layout:     post	
title:      『Computer Architecture』 Single Cycle CPU	
subtitle:   『计算机体系结构』 单周期CPU    
date:       2021-01-17	   
author:     Coekjan 
catalog:    true    
katex:  true    
tags:	
    - Computer Architecture  
---

本节将构造一个支持指令集{`addu`, `subu`, `addiu`, `lui`, `ori`, `lw`, `sw`, `beq`, `j`}的单周期CPU.

## MIPS指令特征

### 三大类指令

MIPS指令均为32位的01串, 根据这32位的功能划分, 可以粗略得到三大指令类型: R型, I型, J型.

**R型**

R型指令的编码如下:

$31\qquad26$ | $25\qquad21$ | $20\qquad16$ | $15\qquad11$ | $10\qquad6$| $5\qquad0$
:-:|:-:|:-:|:-:|:-:|:-:
**opcode**|**rs**|**rt**|**rd**|**shamt**|**funct**
$6$ | $5$ | $5$ | $5$ | $5$ | $6$

> R 型的 opcode 为 `000000` ; 使用不同的 funct 用以区分不同的 R 型指令.

**I型**

$31\qquad26$ | $25\qquad21$ | $20\qquad16$ | $15\qquad\qquad\qquad\qquad\qquad\qquad\qquad\qquad0$ 
:-:|:-:|:-:|:-:
**opcode**|**rs**|**rt**|**imm16**
$6$ | $5$ | $5$ | $16$

**J型**

$31\qquad26$ | $25\qquad\qquad\qquad\qquad\qquad\qquad\qquad\qquad\qquad\qquad\qquad\qquad\qquad0$
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
`j` | J | $$\rm PC\leftarrow PC_{31\dots28}\:\vert\vert\:imm26\:\vert\vert\:0^2$$ | 将imm26**低位补两位0**, 并**高位拼接PC[31:28]**, 得到新PC值, **存入PC**寄存器.

> 我们须找到指令的共性, 从而得知系统的关键部件.

## 关键部件

从上述对MIPS指令的特征分析, 我们指出, 系统中必须包含如下的关键部件.

### PC (Program Counter)

程序计数器: 引导指令存储器读出指令.

![]({{ '/img/CPU-PC.svg' | prepend: site.baseurl}})

### IM (Instruction Memory)

指令存储器: 字节编址, 存储指令的只读模块, 输出的指令由地址端支配.

![]({{ '/img/CPU-IM.svg' | prepend: site.baseurl}})

### NPC (Next PC)

Next PC的计算模块: 用于计算下一条指令的PC.

![]({{ '/img/CPU-NPC.svg' | prepend: site.baseurl}})

### GRF (General Register File)

通用寄存器文件: 内置32个通用寄存器(其中0号寄存器恒为0), 外部支持同时读取两个寄存器, 写一个寄存器.

![]({{ '/img/CPU-GRF.svg' | prepend: site.baseurl}})

### EXT (Extender)

扩展器: 支持零扩展与符号扩展.

![]({{ '/img/CPU-EXT.svg' | prepend: site.baseurl}})

### ALU(Arithmetic Logic Unit)

算术逻辑单元: 支持加, 减, 按位或, 高位加载, 异或.

> 异或操作用于 `beq` 的"相等"判定.

![]({{ '/img/CPU-ALU.svg' | prepend: site.baseurl}})

### DM (Data Memory)

数据存储器: 字节编址, 读写数据宽为一字.

![]({{ '/img/CPU-DM.svg' | prepend: site.baseurl}})

## 数据通路构造

为构造数据通路, 我们必须分析指令集对通路的要求.

### 指令集分析

下面对指令集{`addu`, `subu`, `addiu`, `lui`, `ori`, `lw`, `sw`, `beq`, `j`}中部分指令所需的通路连接进行分析.

> 其中, `subu` 与 `addu` 所需路径一致; `lui` , `ori` 与 `addiu` 所需路径一致.

指令 | `addu` | `addiu` | `lw` | `sw` | `beq` | `j`
:-: | :-: | :-: | :-: | :-: | :-: | :-:
`PC.Next` | `NPC.NextPC` | `NPC.NextPC` | `NPC.NextPC` | `NPC.NextPC` | `NPC.NextPC` | `NPC.NextPC`
`IM.Addr` | `PC.PC` | `PC.PC` | `PC.PC` | `PC.PC` | `PC.PC` | `PC.PC`
`GRF.Addr1` | `IM.Instr[rs]` | `IM.Instr[rs]` |  `IM.Instr[rs]` | `IM.Instr[rs]` | `IM.Instr[rs]` | -
`GRF.Addr2` | `IM.Instr[rt]` | - | - | `IM.Instr[rt]` | `IM.Instr[rt]` | -
`EXT.In16` | - | `IM.Instr[imm16]` | `IM.Instr[imm16]` | `IM.Instr[imm16]` | - | -
`ALU.SrcA` | `GRF.RData1` | `GRF.RData1` | `GRF.RData1` | `GRF.RData1` | `GRF.RData1` | -
`ALU.SrcB` | `GRF.RData2` | `EXT.Out32` | `EXT.Out32` | `EXT.Out32` | `GRF.RData2` | -
`DM.Addr` | - | - | `ALU.Out` | `ALU.Out` | - | -
`DM.WData` | - | - | - | `GRF.RData2` | - | -
`GRF.Addr3` | `IM.Instr[rd]` | `IM.Instr[rd]` | `IM.Instr[rt]` | - | - | -
`GRF.WData`| `ALU.Out` | `ALU.Out` | `DM.RData` | - | - | -

这样, 我们横向合并, 即可得到某输入端口所有的数据来源. 如: `ALU.SrcB` 端的来源有: `GRF.RData2` , `EXT.Out32` .

若某端口的输入来源只有一个, 那么只需直接连接来源即可; 若某端口的输入来源不止一个, 那么就必须引入多路选择器, 并增加一个控制信号以选择数据.

### 通路形成

> 这里将IM与DM分离出数据通路.

![]({{ '/img/S-CPU-DP.svg' | prepend: site.baseurl}})

## 控制器

从数据通路中, 可以看出通路需要以下控制信号:
* `PC.En`
* `NPC.Ctrl`
* `GRF.WEn`
* `EXT.Ctrl`
* `ALU.Ctrl`
* `DM.WEn`
* 3个的多选器的选择信号.

为产生这些信号, 我们需要得到指令的opcode与funct进行分析. Verilog实现的部分代码如下:

```verilog
/* Identify */
wire addu   = (opcode == 6'b000000) && (funct == 6'b100001);
wire subu   = (opcode == 6'b000000) && (funct == 6'b100011);
// ...
wire j      = (opcode == 6'b000010);

/* Generate */
assign PC_En        =   1; // Always update PC
assign NPC_Ctrl     =   beq     ? `BrEq :
                        j       ? `Jmp  : `Add4;
assign GRF_WEn      =   addu | subu | addiu | lui | ori | lw;
//...
```

## 系统组装

![]({{ '/img/S-CPU.svg' | prepend: site.baseurl}})

## 系统测试

### 样例

单周期CPU中, 前后指令的指令不冲突, 因此只需进行正确性测试.

### 验证

根据系统论, 某系统若能分解为若干个子系统以及它们之间的关联方式, 则只需要证明子系统与关联方式正确, 则可以断言该系统正确. 而验证子系统只需要继续按该思路分解并验证.

> 这一分解过程是有尽头的, 若分解到某一显然正确的部件, 那么就不需要继续往下分解.

## 系统增量开发

本节仅考虑了9条简单的指令, 如果需要对指令规模进行增量开发, 则可以按照以下步骤进行:
1. RTL分析: 根据新指令RTL, 若已有部件可以支持新指令, 则直接进行指令集分析; 否则则需要为新指令增设新部件.
2. 指令集分析: 在已有的[指令集分析表格](#指令集分析)中继续增加待添加指令, 并重新合并行, 得到所有输入端口的所有数据来源.
3. 通路增量开发: 根据指令集分析, 增加必要的MUX.
4. 控制器增量开发: 根据指令集分析, 增加必要的控制信号.
5. 针对新指令进行测试.
