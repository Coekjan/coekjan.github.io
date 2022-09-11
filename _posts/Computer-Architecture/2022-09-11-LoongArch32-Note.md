---	
layout:     post	
title:      『Computer Architecture』 LoongArch32 (Reduced) Note	
subtitle:   『计算机体系结构』 龙架构 32 位精简体系的笔记    
date:       2022-09-11	   
author:     Coekjan 
header-img: img/post-bg-CA.jpg	
catalog:    true	
katex:    true 
tags:	
    - Computer Architecture  
---

> 本博客主要是为了精确地整理出 LoongArch32 (Reduced) 体系下的知识

## 存取地址与相关例外

MMU 接受一个地址，将根据 CPU 的状态，直接使用该地址或转化为另一地址后访问物理地址（可能访问内存，也可能访问 MMIO 的外设）。其具体逻辑可由下述伪代码描述：

$$
\begin{aligned}
    \tt proc ~~& \tt access(type, addr, size, plv, map, asid, buf): \\
                &
                \begin{aligned}
                    \tt if ~~~ & \tt addr ~~\%~~ size \ne 0: \\
                    & \tt SignalException(type = IF ~~?~~ ADEF : ALE) \\
                    & \tt exit
                \end{aligned} \\
                &
                \begin{aligned}
                    \tt if ~~~ & \tt not ~~~ map: \\
                    &
                    \begin{aligned}
                        \tt case ~~~ & \tt type: \\
                        & \tt IF: ~~ bus[addr:addr+size] \xrightarrow{CSR.CRMD.DATF} buf \\
                        & \tt LD: ~~ bus[addr:addr+size] \xrightarrow{CSR.CRMD.DATM} buf \\
                        & \tt ST: ~~ bus[addr:addr+size] \xleftarrow{CSR.CRMD.DATM} buf \\
                    \end{aligned} \\
                    & \tt exit
                \end{aligned} \\
                &
                \begin{aligned}
                    \tt for ~~~ & \tt dmw ~~in~~ \{CSR.DMW0, CSR.DMW1\}:\\
                    &
                    \begin{aligned}
                        \tt if ~~~ & \tt addr[31:29] = dmw.VSEG ~~and~~ ( \\
                        & \tt (plv = PLV0 ~~and~~ dmw.PLV0 = 1) ~~or~~ \\
                        & \tt (plv = PLV3 ~~and~~ dmw.PLV3 = 1)): \\
                        & \tt paddr = \{dmw.PSEG,addr[28:0]\} \\
                        &
                        \begin{aligned}
                            \tt case ~~~ & \tt type: \\
                            & \tt IF: ~~ bus[paddr:paddr+size] \xrightarrow{dmw.MAT} buf \\
                            & \tt LD: ~~ bus[paddr:paddr+size] \xrightarrow{dmw.MAT} buf \\
                            & \tt ST: ~~ bus[paddr:paddr+size] \xleftarrow{dmw.MAT} buf \\
                        \end{aligned} \\
                        & \tt exit
                    \end{aligned} \\
                \end{aligned} \\
                &
                \begin{aligned}
                    \tt if ~~~ & \tt plv = PLV3 ~~and~~ addr \ge 0x80000000: \\
                    & \tt SignalException(type = IF ~~?~~ ADEF : ADEM) \\
                    & \tt exit
                \end{aligned} \\
                & \tt found = false \\
                & \tt found\_v = SomeVal \\
                & \tt found\_d = SomeVal \\
                & \tt found\_mat = SomeVal \\
                & \tt found\_plv = SomeVal \\
                & \tt found\_ppn = SomeVal \\
                & \tt found\_ps = SomeVal \\
                &
                \begin{aligned}
                    \tt for ~~~ & \tt ent ~~in~~ TLB: \\
                    &
                    \begin{aligned}
                        \tt if ~~~  & \tt ent.E = 1 ~~and~~ \\
                                    & \tt (ent.G = 1 ~~or~~ ent.ASID = asid) ~~and~~ \\
                                    & \tt ent.VPPN[..ent.PS-12] = addr[31:ent.PS+1]:\\
                                    &
                                    \begin{aligned}
                                        \tt if ~~~~~~ & \tt not ~~ found: \\
                                        &
                                        \begin{aligned}
                                            & \tt found = true \\
                                            & \tt found\_ps = ent.PS \\
                                            &
                                            \begin{aligned}
                                                \tt if ~~~~~~ & \tt addr[ent.PS] = 0: \\
                                                & \tt found\_v = ent.V0 \\
                                                & \tt found\_d = ent.D0 \\
                                                & \tt found\_mat = ent.MAT0 \\
                                                & \tt found\_plv = ent.PLV0 \\
                                                & \tt found\_ppn = ent.PPN0 \\
                                                \tt else:& \\
                                                & \tt found\_v = ent.V1 \\
                                                & \tt found\_d = ent.D1 \\
                                                & \tt found\_mat = ent.MAT1 \\
                                                & \tt found\_plv = ent.PLV1 \\
                                                & \tt found\_ppn = ent.PPN1
                                            \end{aligned}
                                        \end{aligned} \\
                                        \tt else:&\\
                                        & \tt UNPREDICTABLE
                                    \end{aligned}
                    \end{aligned}
                \end{aligned} \\
                &
                \begin{aligned}
                    \tt if ~~~ & \tt not ~~ found: \\
                    & \tt SignalException(TLBR) \\
                    & \tt exit
                \end{aligned} \\
                &
                \begin{aligned}
                    \tt if ~~~ & \tt not ~~ found\_v: \\
                    &
                    \begin{aligned}
                        \tt case ~~~ & \tt type: \\
                        & \tt IF: ~~ SignalException(PIF) \\
                        & \tt LD: ~~ SignalException(PIL) \\
                        & \tt ST: ~~ SignalException(PIS) \\
                    \end{aligned} \\
                    & \tt exit
                \end{aligned} \\
                &
                \begin{aligned}
                    \tt if ~~~ & \tt plv > found\_plv: \\
                    & \tt SignalException(PPI) \\
                    & \tt exit
                \end{aligned} \\
                &
                \begin{aligned}
                    \tt if ~~~ & \tt type = ST ~~and~~ found\_d = 0: \\
                    & \tt SignalException(PME) \\
                    & \tt exit
                \end{aligned} \\
                & \tt paddr = \{found\_ppn[..found\_ps-12],addr[found\_ps-1:0]\} \\
                &
                \begin{aligned}
                    \tt case ~~~ & \tt type: \\
                    & \tt IF: ~~ bus[paddr:paddr+size] \xrightarrow{found\_mat} buf \\
                    & \tt LD: ~~ bus[paddr:paddr+size] \xrightarrow{found\_mat} buf \\
                    & \tt ST: ~~ bus[paddr:paddr+size] \xleftarrow{found\_mat} buf \\
                \end{aligned} \\
\end{aligned}
$$

## 指令集 RTL 描述

在下述表达中，如果 RTL 执行过程中遇到 $\tt SignalException$ 则应终止后续操作，处理器应响应对应例外。比如访存过程中遇到 $\tt SignalException$，则后续写入寄存器等操作均不应被执行。

注意，下面仅列出了可以用 RTL 语句表达的指令。
- 部分与 Cache 有关的指令不便以 RTL 形式表达，因此这些指令没有被列出；
- `IDLE` 指令不便以 RTL 形式表达，因此没有列出。

### 基础整数指令

#### 算术运算类指令

##### `ADD.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000100000$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt tmp = & \tt GPR[rj] + GPR[rk] \\
    \tt GPR[rd] \leftarrow & \tt tmp[31:0]
\end{aligned}
$$

##### `SUB.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000100010$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt tmp = & \tt GPR[rj] - GPR[rk] \\
    \tt GPR[rd] \leftarrow & \tt tmp[31:0]
\end{aligned}
$$

##### `ADDI.W`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0000001010$ | $\tt si12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt tmp = & \tt GPR[rj] + SignExtend(si12, 32) \\
    \tt GPR[rd] \leftarrow & \tt tmp[31:0]
\end{aligned}
$$

##### `LU12I.W`

| $31~~~~~~~~~~~~~~~25$ | $24~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0001010$ | $\tt si20$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow & \tt \{si20, 12'b0\}
\end{aligned}
$$

##### `SLT`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000100100$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow & \tt (signed(GPR[rj]) < signed(GPR[rk])) ~~?~~ 1 : 0
\end{aligned}
$$

##### `SLTU`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000100101$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow & \tt (unsigned(GPR[rj]) < unsigned(GPR[rk])) ~~?~~ 1 : 0
\end{aligned}
$$

##### `SLTI`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0000001000$ | $\tt si12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt tmp = & \tt SignExtend(si12, 32)\\
    \tt GPR[rd] \leftarrow & \tt (signed(GPR[rj]) < signed(tmp)) ~~?~~ 1 : 0
\end{aligned}
$$

##### `SLTUI`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0000001001$ | $\tt si12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt tmp = & \tt SignExtend(si12, 32)\\
    \tt GPR[rd] \leftarrow & \tt (unsigned(GPR[rj]) < unsigned(tmp)) ~~?~~ 1 : 0
\end{aligned}
$$

##### `PCADDU12I`

| $31~~~~~~~~~~~~~~~25$ | $24~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0001110$ | $\tt si20$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow & \tt PC + SignExtend(\{si20,12'b0\},32)
\end{aligned}
$$

##### `AND`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000101001$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow & \tt GPR[rj] ~~AND~~ GPR[rk]
\end{aligned}
$$

##### `OR`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000101010$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow & \tt GPR[rj] ~~OR~~ GPR[rk]
\end{aligned}
$$

##### `NOR`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000101000$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow & \tt GPR[rj] ~~NOR~~ GPR[rk]
\end{aligned}
$$

##### `XOR`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000101011$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow & \tt GPR[rj] ~~XOR~~ GPR[rk]
\end{aligned}
$$

##### `ANDI`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0000001101$ | $\tt ui12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow & \tt GPR[rj] ~~AND~~ ZeroExtend(ui12, 32)
\end{aligned}
$$

> 空操作 `NOP` 是 `ANDI r0, r0, 0` 的别名

##### `ORI`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0000001110$ | $\tt ui12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow & \tt GPR[rj] ~~OR~~ ZeroExtend(ui12, 32)
\end{aligned}
$$

##### `XORI`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0000001111$ | $\tt ui12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow & \tt GPR[rj] ~~XOR~~ ZeroExtend(ui12, 32)
\end{aligned}
$$

##### `MUL.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000111000$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt prod \leftarrow & \tt signed(GPR[rj]) ~~*~~ signed(GPR[rk]) \\
    \tt GPR[rd] \leftarrow & \tt product[31:0]
\end{aligned}
$$

##### `MULH.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000111001$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt prod \leftarrow & \tt signed(GPR[rj]) ~~*~~ signed(GPR[rk]) \\
    \tt GPR[rd] \leftarrow & \tt product[63:32]
\end{aligned}
$$

##### `MULH.WU`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000111010$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt prod \leftarrow & \tt unsigned(GPR[rj]) ~~*~~ unsigned(GPR[rk]) \\
    \tt GPR[rd] \leftarrow & \tt product[63:32]
\end{aligned}
$$

##### `DIV.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000001000000$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt quo \leftarrow & \tt signed(GPR[rj]) ~~/~~ signed(GPR[rk]) \\
    \tt GPR[rd] \leftarrow & \tt quo[31:0]
\end{aligned}
$$

##### `DIV.WU`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000001000010$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt quo \leftarrow & \tt unsigned(GPR[rj]) ~~/~~ unsigned(GPR[rk]) \\
    \tt GPR[rd] \leftarrow & \tt quo[31:0]
\end{aligned}
$$

##### `MOD.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000001000001$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt rem \leftarrow & \tt signed(GPR[rj]) ~~\%~~ signed(GPR[rk]) \\
    \tt GPR[rd] \leftarrow & \tt rem[31:0]
\end{aligned}
$$

##### `MOD.WU`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000001000011$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt rem \leftarrow & \tt unsigned(GPR[rj]) ~~\%~~ unsigned(GPR[rk]) \\
    \tt GPR[rd] \leftarrow & \tt rem[31:0]
\end{aligned}
$$

#### 移位运算类指令

##### `SLL.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000101110$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt tmp = & \tt SLL(GPR[rj], GPR[rk][4:0]) \\
    \tt GPR[rd] \leftarrow & \tt tmp[31:0]
\end{aligned}
$$

##### `SRL.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000101111$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt tmp = & \tt SRL(GPR[rj], GPR[rk][4:0]) \\
    \tt GPR[rd] \leftarrow & \tt tmp[31:0]
\end{aligned}
$$

##### `SRA.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000000110000$ | $\tt rk$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt tmp = & \tt SRA(GPR[rj], GPR[rk][4:0]) \\
    \tt GPR[rd] \leftarrow & \tt tmp[31:0]
\end{aligned}
$$

##### `SLLI.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000010000001$ | $\tt ui5$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt tmp = & \tt SLL(GPR[rj], ui5) \\
    \tt GPR[rd] \leftarrow & \tt tmp[31:0]
\end{aligned}
$$

##### `SRLI.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000010001001$ | $\tt ui5$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt tmp = & \tt SRL(GPR[rj], ui5) \\
    \tt GPR[rd] \leftarrow & \tt tmp[31:0]
\end{aligned}
$$

##### `SRAI.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000000010010001$ | $\tt ui5$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt tmp = & \tt SRA(GPR[rj], ui5) \\
    \tt GPR[rd] \leftarrow & \tt tmp[31:0]
\end{aligned}
$$

#### 转移指令

##### `BEQ`

| $31~~~~~~~~~~~~~~~26$ | $25~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 010110$ | $\tt offs16$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt if ~~ & \tt GPR[rj] = GPR[rd]: \\
              & \tt PC \leftarrow PC + SignExtend(\{offs16, 2'b0\}, 32)
\end{aligned}
$$

##### `BNE`

| $31~~~~~~~~~~~~~~~26$ | $25~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 010111$ | $\tt offs16$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt if ~~ & \tt GPR[rj] \ne GPR[rd]: \\
              & \tt PC \leftarrow PC + SignExtend(\{offs16, 2'b0\}, 32)
\end{aligned}
$$

##### `BLT`

| $31~~~~~~~~~~~~~~~26$ | $25~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 011000$ | $\tt offs16$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt if ~~ & \tt signed(GPR[rj]) \lt signed(GPR[rd]): \\
              & \tt PC \leftarrow PC + SignExtend(\{offs16, 2'b0\}, 32)
\end{aligned}
$$

##### `BGE`

| $31~~~~~~~~~~~~~~~26$ | $25~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 011001$ | $\tt offs16$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt if ~~ & \tt signed(GPR[rj]) \ge signed(GPR[rd]): \\
              & \tt PC \leftarrow PC + SignExtend(\{offs16, 2'b0\}, 32)
\end{aligned}
$$

##### `BLTU`

| $31~~~~~~~~~~~~~~~26$ | $25~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 011010$ | $\tt offs16$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt if ~~ & \tt unsigned(GPR[rj]) \lt unsigned(GPR[rd]): \\
              & \tt PC \leftarrow PC + SignExtend(\{offs16, 2'b0\}, 32)
\end{aligned}
$$

##### `BGEU`

| $31~~~~~~~~~~~~~~~26$ | $25~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 011011$ | $\tt offs16$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt if ~~ & \tt unsigned(GPR[rj]) \ge unsigned(GPR[rd]): \\
              & \tt PC \leftarrow PC + SignExtend(\{offs16, 2'b0\}, 32)
\end{aligned}
$$

##### `B`

| $31~~~~~~~~~~~~~~~26$ | $25~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|
| $\tt 010100$ | $\tt offs26[15:0]$ | $\tt offs26[25:16]$ |

$$
\begin{aligned}
    \tt PC \leftarrow PC + SignExtend(\{offs26, 2'b0\}, 32)
\end{aligned}
$$

##### `BL`

| $31~~~~~~~~~~~~~~~26$ | $25~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|
| $\tt 010101$ | $\tt offs26[15:0]$ | $\tt offs26[25:16]$ |

$$
\begin{aligned}
    \tt GPR[1] \leftarrow & \tt PC + 4\\
    \tt PC \leftarrow & \tt PC + SignExtend(\{offs26, 2'b0\}, 32)
\end{aligned}
$$

##### `JIRL`

| $31~~~~~~~~~~~~~~~26$ | $25~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 010011$ | $\tt offs16$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow & \tt PC + 4\\
    \tt PC \leftarrow & \tt GPR[rj] + SignExtend(\{offs16, 2'b0\}, 32)
\end{aligned}
$$

#### 普通访存指令

##### `LD.B`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0010100000$ | $\tt si12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    & \tt addr = GPR[rj] + SignExtend(si12, 32)\\
    & \tt buf = ref ~~ byte \\
    & \tt access(LD, addr, 1, CSR.CRMD.PLV, CSR.CRMD.PG,CSR.ASID.ASID,buf) \\
    & \tt GPR[rd] \leftarrow SignExtend(buf, 32)
\end{aligned}
$$

##### `LD.H`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0010100001$ | $\tt si12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    & \tt addr = GPR[rj] + SignExtend(si12, 32)\\
    & \tt buf = ref ~~ half \\
    & \tt access(LD, addr, 2, CSR.CRMD.PLV, CSR.CRMD.PG,CSR.ASID.ASID,buf) \\
    & \tt GPR[rd] \leftarrow SignExtend(buf, 32)
\end{aligned}
$$

##### `LD.W`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0010100010$ | $\tt si12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    & \tt addr = GPR[rj] + SignExtend(si12, 32)\\
    & \tt buf = ref ~~ word \\
    & \tt access(LD, addr, 4, CSR.CRMD.PLV, CSR.CRMD.PG,CSR.ASID.ASID,buf) \\
    & \tt GPR[rd] \leftarrow buf
\end{aligned}
$$

##### `LD.BU`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0010101000$ | $\tt si12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    & \tt addr = GPR[rj] + SignExtend(si12, 32)\\
    & \tt buf = ref ~~ byte \\
    & \tt access(LD, addr, 1, CSR.CRMD.PLV, CSR.CRMD.PG,CSR.ASID.ASID,buf) \\
    & \tt GPR[rd] \leftarrow ZeroExtend(buf, 32)
\end{aligned}
$$

##### `LD.HU`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0010101001$ | $\tt si12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    & \tt addr = GPR[rj] + SignExtend(si12, 32)\\
    & \tt buf = ref ~~ half \\
    & \tt access(LD, addr, 2, CSR.CRMD.PLV, CSR.CRMD.PG,CSR.ASID.ASID,buf) \\
    & \tt GPR[rd] \leftarrow ZeroExtend(buf, 32)
\end{aligned}
$$

##### `ST.B`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0010100100$ | $\tt si12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    & \tt addr = GPR[rj] + SignExtend(si12, 32)\\
    & \tt buf = ref ~~ GPR[rd][7:0] \\
    & \tt access(ST, addr, 1, CSR.CRMD.PLV, CSR.CRMD.PG,CSR.ASID.ASID,buf)
\end{aligned}
$$

##### `ST.H`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0010100101$ | $\tt si12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    & \tt addr = GPR[rj] + SignExtend(si12, 32)\\
    & \tt buf = ref ~~ GPR[rd][15:0] \\
    & \tt access(ST, addr, 2, CSR.CRMD.PLV, CSR.CRMD.PG,CSR.ASID.ASID,buf)
\end{aligned}
$$

##### `ST.W`

| $31~~~~~~~~~~~~~~~~~~~~22$ | $21~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 0010100110$ | $\tt si12$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    & \tt addr = GPR[rj] + SignExtend(si12, 32)\\
    & \tt buf = ref ~~ GPR[rd] \\
    & \tt access(ST, addr, 4, CSR.CRMD.PLV, CSR.CRMD.PG,CSR.ASID.ASID,buf)
\end{aligned}
$$

#### 原子访存指令

##### `LL.W`

| $31~~~~~~~~~~~~~~~~~~~~24$ | $23~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00100000$ | $\tt si14$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    & \tt addr = GPR[rj] + SignExtend(\{si14, 2'b0\}, 32)\\
    & \tt buf = ref ~~ word \\
    & \tt access(LD, addr, 4, CSR.CRMD.PLV, CSR.CRMD.PG,CSR.ASID.ASID,buf) \\
    & \tt GPR[rd] \leftarrow buf \\
    & \tt LLBIT \leftarrow 1
\end{aligned}
$$

##### `SC.W`

| $31~~~~~~~~~~~~~~~~~~~~24$ | $23~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00100000$ | $\tt si14$ | $\tt rj$ | $\tt rd$ |

$$
\begin{aligned}
    \tt if ~~~~~~ & \tt LLBIT = 1: \\
    & \tt addr = GPR[rj] + SignExtend(\{si14, 2'b0\}, 32)\\
    & \tt buf = ref ~~ GPR[rd] \\
    & \tt access(LD, addr, 4, CSR.CRMD.PLV, CSR.CRMD.PG,CSR.ASID.ASID,buf) \\
    & \tt LLBIT \leftarrow 0 \\
    & \tt GPR[rd] \leftarrow 1 \\
    \tt else:& \\
    & \tt GPR[rd] \leftarrow 0
\end{aligned}
$$

#### 其他杂项指令

##### `SYSCALL`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~0$ |
|:-------------------:|:------:|
| $\tt 00000000001010100$ | $\tt code$ |

$$
\begin{aligned}
    \tt SignalException(SYS)
\end{aligned}
$$

##### `BREAK`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~0$ |
|:-------------------:|:------:|
| $\tt 00000000001010110$ | $\tt code$ |

$$
\begin{aligned}
    \tt SignalException(BRK)
\end{aligned}
$$

##### `RDCNTID.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-------:|
| $\tt 0000000000000000011000$ | $\tt rj$ | $\tt 00000$ |

$$
\begin{aligned}
    \tt GPR[rj] \leftarrow CSR.TID.TID
\end{aligned}
$$

##### `RDCNTVL.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-------:|
| $\tt 0000000000000000011000$ | $\tt 00000$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow StableCounter[31:0]
\end{aligned}
$$

##### `RDCNTVH.W`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-------:|
| $\tt 0000000000000000011001$ | $\tt 00000$ | $\tt rd$ |

$$
\begin{aligned}
    \tt GPR[rd] \leftarrow StableCounter[63:32]
\end{aligned}
$$

### 特权指令

#### CSR 访问指令

注意，任何写入 CSR 的操作，都仅将值写入到可写的位域。

##### `CSRRD`

| $31~~~~~~~~~~~~~~~~~~~~24$ | $23~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000100$ | $\tt num$ | $\tt 00000$ | $\tt rd$ |

$$
\begin{aligned}
    \tt if ~~~~~~ & \tt CSR.CRMD.PLV = PLV0:\\
    & \tt GPR[rd] \leftarrow CSR[num] \\
    \tt else:& \\
    & \tt SignalException(IPE)
\end{aligned}
$$

##### `CSRWR`

| $31~~~~~~~~~~~~~~~~~~~~24$ | $23~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000100$ | $\tt num$ | $\tt 00001$ | $\tt rd$ |

$$
\begin{aligned}
    \tt if ~~~~~~ & \tt CSR.CRMD.PLV = PLV0:\\
    & \tt GPR[rd] \leftarrow CSR[num] \\
    & \tt CSR[num] \leftarrow GPR[rd]\\
    \tt else:& \\
    & \tt SignalException(IPE)
\end{aligned}
$$

##### `CSRXCHG`

| $31~~~~~~~~~~~~~~~~~~~~24$ | $23~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000100$ | $\tt num$ | $\tt rj\ne 0,1$ | $\tt rd$ |

$$
\begin{aligned}
    \tt if ~~~~~~ & \tt CSR.CRMD.PLV = PLV0:\\
    & \tt GPR[rd] \leftarrow CSR[num] \\
    & \tt mask = GPR[rj] \\
    & \tt val = (GPR[rd] ~~AND~~ mask) ~~OR~~ (CSR[num] ~~AND~~NOT~~ mask)\\
    & \tt CSR[num] \leftarrow val\\
    \tt else:& \\
    & \tt SignalException(IPE)
\end{aligned}
$$

#### TLB 维护指令

##### `TLBSRCH`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-------:|
| $\tt 0000011001001000001010$ | $\tt 00000$ | $\tt 00000$ |

$$
\begin{aligned}
    \tt if ~~~~~~ & \tt CSR.CRMD.PLV = PLV0: \\
    &
    \begin{aligned}
        &\tt found = false \\
        &\tt index = SomeVal \\
        &
        \begin{aligned}
            \tt for ~~~ & \tt ent ~~in~~ TLB: \\
            &
            \begin{aligned}
                \tt if ~~~ & \tt ent.E = 1 ~~and~~ \\
                & \tt (ent.G = 1 ~~OR~~ ent.ASID = CSR.ASID.ASID) ~~and~~ \\
                & \tt ent.VPPN[..ent.PS - 12] = CSR.TLBEHI.VPPN[..ent.PS - 12]: \\
                & \tt found = true \\
                & \tt index = TLB.index\_of(ent)
            \end{aligned}
        \end{aligned} \\
        &
        \begin{aligned}
            \tt if ~~~~~~ & \tt found: \\
            & \tt CSR.TLBIDX.NE \leftarrow 0 \\
            & \tt CSR.TLBIDX.INDEX \leftarrow index \\
            \tt else:& \\
            & \tt CSR.TLBIDX.NE \leftarrow 1
        \end{aligned}
    \end{aligned} \\
    \tt else: &\\
    & \tt SignalException(IPE)
\end{aligned}
$$

##### `TLBRD`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-------:|
| $\tt 0000011001001000001011$ | $\tt 00000$ | $\tt 00000$ |

$$
\begin{aligned}
    \tt if ~~~~~~ & \tt CSR.CRMD.PLV = PLV0: \\
    &
    \begin{aligned}
        & \tt ent = TLB[CSR.TLBIDX.INDEX] \\
        &
        \begin{aligned}
            \tt if ~~~~~~ & \tt ent.E = 1:\\
            & \tt CSR.ASID.ASID \leftarrow ent.ASID \\
            & \tt CSR.TLBEHI.VPPN \leftarrow ent.VPPN \\
            & \tt CSR.TLBELO0.PLV \leftarrow ent.PLV0 \\
            & \tt CSR.TLBELO0.MAT \leftarrow ent.MAT0 \\
            & \tt CSR.TLBELO0.PPN \leftarrow ent.PPN0 \\
            & \tt CSR.TLBELO0.G \leftarrow ent.G \\
            & \tt CSR.TLBELO0.D \leftarrow ent.D0 \\
            & \tt CSR.TLBELO0.V \leftarrow ent.V0 \\
            & \tt CSR.TLBELO1.PLV \leftarrow ent.PLV1 \\
            & \tt CSR.TLBELO1.MAT \leftarrow ent.MAT1 \\
            & \tt CSR.TLBELO1.PPN \leftarrow ent.PPN1 \\
            & \tt CSR.TLBELO1.G \leftarrow ent.G \\
            & \tt CSR.TLBELO1.D \leftarrow ent.D1 \\
            & \tt CSR.TLBELO1.V \leftarrow ent.V1 \\
            & \tt CSR.TLBIDX.PS \leftarrow ent.PS \\
            & \tt CSR.TLBIDX.NE \leftarrow 0 \\
            \tt else:& \\
            & \tt CSR.ASID.ASID \leftarrow 0 \\
            & \tt CSR.TLBEHI \leftarrow 0 \\
            & \tt CSR.TLBELO0 \leftarrow 0 \\
            & \tt CSR.TLBELO1 \leftarrow 0 \\
            & \tt CSR.TLBIDX.NE \leftarrow 1 \\
            & \tt CSR.TLBIDX.PS \leftarrow 0 \\
        \end{aligned}
    \end{aligned} \\
    \tt else:& \\
    & \tt SignalException(IPE)
\end{aligned}
$$

##### `TLBWR`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-------:|
| $\tt 0000011001001000001100$ | $\tt 00000$ | $\tt 00000$ |

$$
\begin{aligned}
    \tt if ~~~~~~ & \tt CSR.CRMD.PLV = PLV0: \\
    &
    \begin{aligned}
        & \tt ent = ref ~~ TLB[CSR.TLBIDX.INDEX] \\
        &
        \begin{aligned}
            \tt if ~~~~~~ & \tt CSR.ESTAT.ECODE = 0x3F: \\
            & \tt ent.E \leftarrow 1 \\
            \tt else: & \\
            & \tt ent.E \leftarrow ~~NOT~~ CSR.TLBIDX.NE
        \end{aligned} \\
        & \tt ent.ASID \leftarrow CSR.ASID.ASID \\
        & \tt ent.G \leftarrow CSR.TLBELO0.G ~~AND~~ CSR.TLBELO1.G \\
        & \tt ent.PS \leftarrow CSR.TLBIDX.PS \\
        & \tt ent.VPPN \leftarrow CSR.TLBEHI.VPPN \\
        & \tt ent.V0 \leftarrow CSR.TLBELO0.V \\
        & \tt ent.D0 \leftarrow CSR.TLBELO0.D \\
        & \tt ent.MAT0 \leftarrow CSR.TLBELO0.MAT \\
        & \tt ent.PLV0 \leftarrow CSR.TLBELO0.PLV \\
        & \tt ent.PPN0 \leftarrow CSR.TLBELO0.PPN \\
        & \tt ent.V1 \leftarrow CSR.TLBELO1.V \\
        & \tt ent.D1 \leftarrow CSR.TLBELO1.D \\
        & \tt ent.MAT1 \leftarrow CSR.TLBELO1.MAT \\
        & \tt ent.PLV1 \leftarrow CSR.TLBELO1.PLV \\
        & \tt ent.PPN1 \leftarrow CSR.TLBELO1.PPN \\
    \end{aligned} \\
    \tt else:& \\
    & \tt SignalException(IPE)
\end{aligned}
$$

##### `TLBFILL`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-------:|
| $\tt 0000011001001000001101$ | $\tt 00000$ | $\tt 00000$ |

$$
\begin{aligned}
    \tt if ~~~~~~ & \tt CSR.CRMD.PLV = PLV0: \\
    &
    \begin{aligned}
        & \tt ent = ref ~~ TLB[RANDOM] \\
        &
        \begin{aligned}
            \tt if ~~~~~~ & \tt CSR.ESTAT.ECODE = 0x3F: \\
            & \tt ent.E \leftarrow 1 \\
            \tt else: & \\
            & \tt ent.E \leftarrow ~~NOT~~ CSR.TLBIDX.NE
        \end{aligned} \\
        & \tt ent.ASID \leftarrow CSR.ASID.ASID \\
        & \tt ent.G \leftarrow CSR.TLBELO0.G ~~AND~~ CSR.TLBELO1.G \\
        & \tt ent.PS \leftarrow CSR.TLBIDX.PS \\
        & \tt ent.VPPN \leftarrow CSR.TLBEHI.VPPN \\
        & \tt ent.V0 \leftarrow CSR.TLBELO0.V \\
        & \tt ent.D0 \leftarrow CSR.TLBELO0.D \\
        & \tt ent.MAT0 \leftarrow CSR.TLBELO0.MAT \\
        & \tt ent.PLV0 \leftarrow CSR.TLBELO0.PLV \\
        & \tt ent.PPN0 \leftarrow CSR.TLBELO0.PPN \\
        & \tt ent.V1 \leftarrow CSR.TLBELO1.V \\
        & \tt ent.D1 \leftarrow CSR.TLBELO1.D \\
        & \tt ent.MAT1 \leftarrow CSR.TLBELO1.MAT \\
        & \tt ent.PLV1 \leftarrow CSR.TLBELO1.PLV \\
        & \tt ent.PPN1 \leftarrow CSR.TLBELO1.PPN \\
    \end{aligned} \\
    \tt else:& \\
    & \tt SignalException(IPE)
\end{aligned}
$$

##### `INVTLB`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~15$ | $14~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-----:|:-----:|
| $\tt 00000110010010011$ | $\tt rk$ | $\tt rj$ | $\tt op$ |

$$
\begin{aligned}
    \tt if ~~~~~~ & \tt CSR.CRMD.PLV = PLV0: \\
    &
    \begin{aligned}
        \tt for ~~~ & \tt ref ~~ ent ~~in~~ TLB: \\
        &
        \begin{aligned}
            \tt case ~~~ & \tt  op: \\
            &
            \begin{aligned}
                \tt 0x0: & \\
                & \tt ent = 0 \\
                \tt 0x1: & \\
                & \tt ent = 0 \\
                \tt 0x2: & \\
                &
                \begin{aligned}
                    \tt if ~~~ & \tt ent.G = 1: \\
                    & \tt ent = 0
                \end{aligned} \\
                \tt 0x3: & \\
                &
                \begin{aligned}
                    \tt if ~~~ & \tt ent.G = 0: \\
                    & \tt ent = 0
                \end{aligned} \\
                \tt 0x4: & \\
                &
                \begin{aligned}
                    \tt if ~~~ & \tt ent.G = 0 ~~and~~ ent.ASID = GPR[rj][9:0]: \\
                    & \tt ent = 0
                \end{aligned} \\
                \tt 0x5: &\\
                &
                \begin{aligned}
                    \tt if ~~~ & \tt ent.G = 0 ~~and~~ \\
                    & \tt ent.ASID = GPR[rj][9:0] ~~and~~ \\
                    & \tt ent.VPPN[..ent.PS - 12] = GPR[rk][31:ent.PS + 1]: \\
                    & \tt ent = 0 \\
                \end{aligned} \\
                \tt 0x6: &\\
                &
                \begin{aligned}
                    \tt if ~~~ & (\tt ent.G = 0 ~~or~~ \\
                    & \tt ent.ASID = GPR[rj][9:0]) ~~and~~ \\
                    & \tt ent.VPPN[..ent.PS - 12] = GPR[rk][31:ent.PS + 1]: \\
                    & \tt ent = 0 \\
                \end{aligned}
            \end{aligned}
        \end{aligned}
    \end{aligned} \\
    \tt else:& \\
    & \tt SignalException(IPE)
\end{aligned}
$$

##### `ERTN`

| $31~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~10$ | $9~~~~~~~~~~5$ | $4~~~~~~~~~~0$ |
|:-------------------:|:------:|:-------:|
| $\tt 0000011001001000001110$ | $\tt 00000$ | $\tt 00000$ |

$$
\begin{aligned}
    \tt if ~~~~~~ & \tt CSR.CRMD.PLV = PLV0: \\
    & \tt CSR.CRMD.PLV \leftarrow CSR.PRMD.PPLV \\
    & \tt CSR.CRMD.IE \leftarrow CSR.PRME.PIE \\
    & \tt PC \leftarrow CSR.ERA.PC \\
    &
    \begin{aligned}
        \tt if ~~~~~~ & \tt CSR.ESTAT.ECODE = 0x3F: \\
        & \tt CSR.CRMD.DA \leftarrow 0 \\
        & \tt CSR.CRMD.PG \leftarrow 1 \\
    \end{aligned} \\
    &
    \begin{aligned}
        \tt if ~~~~~~ & \tt CSR.LLBCTL.KLO = 1: \\
        & \tt CSR.LLBCTL.KLO \leftarrow 0 \\
        \tt else: & \\
        & \tt LLBIT \leftarrow 0 \\
    \end{aligned} \\
    \tt else:& \\
    & \tt SignalException(IPE)
\end{aligned}
$$

## 中断与例外处理

中断引发处理器陷入内核态的条件是：

$$
\begin{aligned}
    \tt (CSR.ECFG.LIE ~~AND~~ CSR.ESTAT.IS) ~~and~~ CSR.CRMD.IE
\end{aligned}
$$

即中断输入采样 $\tt CSR.ESTAT.IS$ 与中断输入使能 $\tt CSR.ECFG.LIE$ 按位与的结果非 $\tt 0$，且全局中断使能 $\tt CSR.CRMD.IE$ 有效。

例外处理的流程是：

$$
\begin{aligned}
    & \tt CSR.PRMD.PPLV \leftarrow CSR.CRMD.PLV \\
    & \tt CSR.PRMD.PIE \leftarrow CSR.CRMD.IE \\
    & \tt CSR.CRMD.PLV \leftarrow PLV0 \\
    & \tt CSR.CRMD.IE \leftarrow 0 \\
    & \tt CSR.ERA.PC \leftarrow PC \\
    & \tt CSR.ESTAT.ECODE \leftarrow ecode\_of(SignalException) \\
    & \tt CSR.ESTAT.ESUBCODE \leftarrow esubcode\_of(SignalException) \\
    &
    \begin{aligned}
        \tt if ~~~~~~ & \tt SignalException = TLBR: \\
        & \tt PC \leftarrow \{CSR.TLBRENTRY.PA, 6'b0\} \\
        \tt else:&\\
        & \tt PC \leftarrow \{CSR.EENTRY.VA, 6'b0\}
    \end{aligned} \\
    &
    \begin{aligned}
        \tt if ~~~~~~ & \tt SignalException \in \{TLBR,ADEF,ADEM,ALE,PIL,PIS,PIF,PME,PPI\}:\\
        & \tt CSR.BADV.VADDR \leftarrow \text{the address causing exception}
    \end{aligned} \\
    &
    \begin{aligned}
        \tt if ~~~~~~ & \tt SignalException \in \{TLBR,PIL,PIS,PIF,PME,PPI\}:\\
        & \tt CSR.TLBEHI.VPPN \leftarrow (\text{the address causing exception})[31:13]
    \end{aligned}
\end{aligned}
$$
