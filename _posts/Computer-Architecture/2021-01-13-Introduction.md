---	
layout:     post	
title:      『Computer Architecture』 Introduction	
subtitle:   『计算机体系结构』 引言    
date:       2021-01-13	   
author:     Coekjan 
header-img: img/post-bg-CA.jpg	
catalog:    true	
non-ligatures: true 
tags:	
    - Computer Architecture  
---

## 预备知识

* 逻辑系统, 组合逻辑, 时序逻辑, 有限状态机
* **MIPS**汇编语言
* 高级程序设计语言(如: **C/C++/C#/Java/Python**), 将用于自动化测试工具的实现, 本系列文章将使用**Python3**
* 硬件描述语言(如: **VerilogHDL/VHDL/SystemVerilog**), 本系列文章将使用**VerilogHDL**

## 预期目标

### CPU架构与指令集

预期实现经典的五级流水线架构.

#### 指令集(仅支持定点指令)

指令序号 | 指令符号 | 指令说明
:-:|:-:|:----
1 | `lb` | 取有符号字节
2 | `lbu` | 取无符号字节
3 | `lh` | 取有符号半字
4 | `lhu` | 取无符号半字
5 | `lw` | 取全字
6 | `sb` | 存字节
7 | `sh` | 存半字
8 | `sw` | 存全字
9 | `add` | 加
10 | `addu` | 加(不检测溢出)
11 | `sub` | 减
12 | `subu` | 减(不检测溢出)
13 | `mult` | 有符号乘
14 | `multu` | 无符号乘
15 | `div` | 有符号除
16 | `divu` | 无符号除
17 | `sll` | 逻辑左移立即数
18 | `srl` | 逻辑右移立即数
19 | `sra` | 算术右移立即数
20 | `sllv` | 逻辑左移
21 | `srlv` | 逻辑右移
22 | `srav` | 算术右移
23 | `and` | 按位与
24 | `or` | 按位或
25 | `xor` | 按位异或
26 | `nor` | 按位或非
27 | `addi` | 加立即数
28 | `addiu` | 加立即数(不检测溢出)
29 | `andi` | 按位与立即数
30 | `ori` | 按位或立即数
31 | `xori` | 按位异或立即数
32 | `lui` | 加载立即数至高位
33 | `slt` | 小于则置位(有符号)
34 | `slti` | 小于立即数则置位(有符号)
35 | `sltiu` | 小于立即数则置位(无符号)
36 | `sltu` | 小于则置位(无符号)
37 | `beq` | 相等则转移
38 | `bne` | 不等则转移
39 | `blez` | 小于等于0则转移
40 | `bgtz` | 大于0则转移
41 | `bltz` | 小于0则转移
42 | `bgez` | 大于等于0则转移
43 | `j` | 无条件跳转至立即数地址
44 | `jal` | 无条件跳转至立即数地址并链接
45 | `jalr` | 无条件跳转至寄存器地址并链接
46 | `jr` | 无条件跳转至寄存器地址
47 | `mfhi` | 从`HI`取值
48 | `mflo` | 从`LO`取值
49 | `mthi` | 向`HI`存值
50 | `mtlo` | 向`LO`存值
51 | `mfc0` | 从`CP0`取值
52 | `mtc0` | 向`CP0`存值
53 | `eret` | 恢复用户态

### 微体系规格

#### 宏观地址

地址所属 | 地址范围 | 备注
:-: | :-: | :-:
数据存储器 | `0x00000000 ~ 0x00002fff` | -
指令存储器 | `0x00003000 ~ 0x00004fff` | 中断处理程序入口地址为`0x00004180`
定时器0 | `0x00007f00 ~ 0x00007f0b` | 内置3个寄存器
定时器1 | `0x00007f10 ~ 0x00007f1b` | 内置3个寄存器

#### 异常与中断码表

<table>
  <tr>
    <th>异常与中断码</th>
    <th>助记符与名称</th>
    <th>指令与指令类型</th>
    <th>描述与备注</th>
  </tr>
  <tr>
    <td align="center">0</td>
    <td align="center"><code>Int</code>(外部中断)</td>
    <td align="center">所有指令</td>
    <td>中断请求</td>
  </tr>
  <tr>
    <td rowspan="7" align="center">4</td>
    <td rowspan="2" align="center"><code>AdEL</code>(取指异常)</td>
    <td rowspan="2" align="center">所有指令</td>
    <td>PC地址未字对齐</td>
  </tr>
  <tr>
    <td>PC地址超过<code>0x3000~0x4ffc</code></td>
  </tr>
  <tr>
    <td rowspan="5" align="center"><code>AdEL</code>(取数异常)</td>
    <td align="center"><code>lw</code></td>
    <td>取数地址未与4字节对齐</td>
  </tr>
  <tr>
    <td align="center"><code>lh</code>, <code>lhu</code></td>
    <td>取数地址未与2字节对齐</td>
  </tr>
  <tr>
    <td align="center"><code>lh</code>, <code>lhu</code>, <code>lb</code>, <code>lbu</code></td>
    <td>取数地址超过数据存储器的范围</td>
  </tr>
  <tr>
    <td align="center">load型指令</td>
    <td>取数地址超过数据存储器与设备范围</td>
  </tr>
  <tr>
    <td align="center">load型指令</td>
    <td>计算地址时加法溢出</td>
  </tr>
  <tr>
    <td rowspan="6" align="center">5</td>
    <td rowspan="6" align="center"><code>AdES</code>(存数异常)</td>
    <td align="center"><code>sw</code></td>
    <td>存数地址未4字节对齐</td>
  </tr>
  <tr>
    <td align="center"><code>sh</code></td>
    <td>存数地址未2字节对齐</td>
  </tr>
  <tr>
    <td align="center"><code>sh</code>, <code>sb</code></td>
    <td>存数地址超过数据存储器与设备范围</td>
  </tr>
  <tr>
    <td align="center">store型指令</td>
    <td>计算地址加法溢出</td>
  </tr>
  <tr>
    <td align="center">store型指令</td>
    <td>向设备只读段存值</td>
  </tr>
  <tr>
    <td align="center">store型指令</td>
    <td>取数地址超过数据存储器与设备范围</td>
  </tr>
  <tr>
    <td align="center">10</td>
    <td align="center"><code>RI</code>(未知指令)</td>
    <td align="center">-</td>
    <td>未知的指令码</td>
  </tr>
  <tr>
    <td align="center">12</td>
    <td align="center"><code>Ov</code>(溢出异常)</td>
    <td align="center"><code>add</code>, <code>addi</code>, <code>sub</code></td>
    <td>算术溢出</td>
  </tr>
</table>

#### 协处理器0

CP0寄存器名称 | 读写 | 寄存器规范
--: | :-: | :--
SR(状态寄存器) | R/W | `IM`(设备中断使能), `EXL`(是否处于内核态), `IE`(全局中断使能)
CAUSE(原因寄存器) | R | `IP`(设备中断请求), `EXC`(异常码), `BD`(分支延迟)
EPC(异常PC寄存器) | R/W | `EPC`(异常返回点)
PR(处理器寄存器) | R | `0xDEAD_C0DE`(自定义型号)

#### 非标准规格

* 指令存储器与数据存储器相分离
* 不实现高速缓存, 并默认指令存储器与数据存储器在一周期内可以完成读写
* 不实现虚拟内存

## 参考软件与本系列博文使用的版本

* **Logisim** 可视化的数字电路搭建与仿真软件
  * [Logisim - 官方网站](http://www.cburch.com/logisim/)
  * 本系列博文将使用 **Logisim 2.7.1**
* **ISE Design Suite** Verilog的开发与仿真软件
  * [ISE - 官方网站](https://china.xilinx.com/support/download/index.html/content/xilinx/zh/downloadNav/vivado-design-tools/archive-ise.html)
  * 本系列博文将使用 **ISE Design Suite 14.7**
* **MARS** MIPS汇编与仿真软件
  * [MARS - 官方网站](http://courses.missouristate.edu/kenvollmar/mars/tutorial.htm)
  * 本系列博文将使用 **MARS 4.5**
