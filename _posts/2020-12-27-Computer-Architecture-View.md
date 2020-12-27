---	
layout:     post	
title:      『Computer Architecture』 Introduction	
subtitle:   『计算机体系结构』 引言    
date:       2020-12-27	   
author:     Coekjan 
header-img: img/post-bg-CA.jpg	
catalog:    true	
tags:	
    - Computer Architecture  
---

### 简介

本系列博客文章将以**Computer Organization**(*北航计算机学院核心专业课*)为基础, 从单周期CPU开始, 搭建一个内含流水线CPU的, 支持I/O与异常和中断的MIPS微体系.

### 前言

#### 预备知识

* 逻辑系统, 组合逻辑, 时序逻辑, 有限状态机
* 一种高级程序设计语言(如: **C/C++/C#/Java/Python**), 将用于自动化测试工具的实现, 本系列文章将使用**Python**
* 一种**RISC**汇编语言(如: **MIPS32**), 本系列文章将使用**MIPS32**
* 一种硬件描述语言(如: **VerilogHDL/VHDL/SystemVerilog**), 本系列文章将使用**VerilogHDL**

#### 参考软件与本系列博文使用的版本

* **Logisim** 可视化的数字电路搭建与仿真软件
  * [Logisim - 官方网站](http://www.cburch.com/logisim/)
  *  **Logisim 2.7.1**
* **ISE Design Suite** Verilog的开发与仿真软件
  * [ISE - 官方网站](https://china.xilinx.com/support/download/index.html/content/xilinx/zh/downloadNav/vivado-design-tools/archive-ise.html)
  * **ISE Design Suite 14.7**
* **MARS** MIPS汇编与仿真软件
  * [MARS - 官方网站](http://courses.missouristate.edu/kenvollmar/mars/tutorial.htm)
  * **MARS 4.5**

### 系列博文

#### MIPS-CPU自动化测试工具

#### MIPS-CPU单周期理论与设计

#### MIPS-CPU单周期初步搭建与测试

#### MIPS-CPU流水线理论与设计

#### MIPS-CPU流水线初步搭建与测试

#### MIPS-CPU流水线增量开发与测试

#### MIPS-MicroSystem协处理器0

#### MIPS-MicroSystem地址空间与异常/中断

#### MIPS-MicroSystem系统桥与设备I/O

#### MIPS-MicroSystem测试