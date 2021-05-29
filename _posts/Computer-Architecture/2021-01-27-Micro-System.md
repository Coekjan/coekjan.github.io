---	
layout:     post	
title:      『Computer Architecture』 Micro System	
subtitle:   『计算机体系结构』 微系统    
date:       2021-01-27	   
author:     Coekjan 
header-img: img/post-bg-CA.jpg	
catalog:    true    
katex:  true    
non-ligatures: true 
tags:	
    - Computer Architecture  
---

## 异常

### 异常指令

考察如下指令: 
```powershell
lw      $1, 1($0)
```

该指令从内存中读取一个整字, 但是其访问的地址并非4的整数倍(不对齐). 我们把这样的指令称为异常指令. 类似的异常在引言中有提到, 包括: `AdEL`, `AdES`, `RI`, `Ov`等.
 
下面我们需要引入异常处理程序来处理异常, 并改造流水线来引导CPU进入与退出异常处理程序.

### 异常处理程序

为处理异常指令, 我们需要引入异常处理程序. 异常处理程序也存储于IM中, 入口地址为`0x00004180`.

> 本设计中默认异常处理程序中无异常指令.

基本编写方法如下: 
```powershell
.ktext 0x00004180

######################
#  handle exception  #
######################

eret # exit handler
```

> 如何处理异常是软件工程师需要考虑的事情, 而不是硬件工程师需要考虑的.


### 协处理器0(CP0)

为帮助程序员分析异常, 须在流水线中加入协处理器0. 

引言中, 给出了协处理器0的内置寄存器: 
* PR寄存器(用于存储处理器型号, 可随意实现)
* SR寄存器(用于存储处理器状态)
  * IM(用于指示外部中断使能, 暂时不实现)
  * IE(用于指示全局中断使能, 暂时不实现)
  * EXL(当且仅当处理器处于异常处理程序中时, EXL为1)
* CAUSE寄存器将提供异常码来告知程序员发生了什么异常: 
  * IP(用于记录外部中断信号, 暂时不实现)
  * BD(当且仅当异常指令位于延迟槽时, BD为1)
  * EXC(用于记录异常指令的异常码)
* EPC寄存器将存储发生异常的指令地址, 引导CPU执行完异常处理程序后恢复到用户代码段

#### 异常码

为记录异常, 硬件工程师需要与软件工程师达成协议: 使用一套统一的异常码来记录.

<table>
  <tr>
    <th>异常与中断码</th>
    <th>助记符与名称</th>
    <th>指令与指令类型</th>
    <th>描述与备注</th>
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
    <td align="center">load 型指令</td>
    <td>取数地址超过数据存储器与设备范围</td>
  </tr>
  <tr>
    <td align="center">load 型指令</td>
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
    <td align="center">store 型指令</td>
    <td>计算地址加法溢出</td>
  </tr>
  <tr>
    <td align="center">store 型指令</td>
    <td>向设备只读段存值</td>
  </tr>
  <tr>
    <td align="center">store 型指令</td>
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

通过观察上述异常码表, 不难知道检测异常指令只需:

```verilog
wire ExceptionIntRq = |EXCCode; // EXCCode != 5'b0
```

响应异常:

```verilog
assign Irq = ExceptionIntRq;
```

> 单目 `|` 运算符为缩位运算符, 功能是对数据所有位进行按位或, 得到的结果为1位数据

##### SR寄存器 - EXL

当检测到异常指令时, SR寄存器将EXL段置1, 表示进入异常处理程序；当流水线检测到`eret`时, SR寄存器将EXL置0, 表示退出异常处理程序.

```verilog
reg [31:0] _Sr; // Read & Write

always @(posedge Clk) begin
    if (Rst) begin
        // reset SR-Register
    end else begin
        if (EXL_Rst) _Sr <= 1'b0;
        if (WEn && Addr == `SR_Addr) begin
            _Sr <= Value;
        end else if (Irq) begin
            _Sr[1] <= 1'b1;
        end
    end
end

assign RData = (Addr == `SR_Addr) ? _Sr : // ...
```

##### CAUSE寄存器 - BD, EXC

当检测到异常指令时, CAUSE寄存器将异常码存入EXC段, 若异常指令位于延迟槽, 则需要把BD段置1:

```verilog
reg [31:0] _Cause; // Only-read

always @(posedge Clk) begin
    if (Rst) begin
        // reset CAUSE-Register
    end else begin
        if (Irq) begin
            _Cause[31] <= BD; // Branch-Delay
            _Cause[6:2] <= EXCCode; // Exception-code
        end
    end
end

assign RData = (Addr == `CAUSE_Addr) ? _Cause : // ...
```

程序员可以通过`mfc0`读取CAUSE寄存器值, 随后可以通过掩码, 移位等操作取得异常码与BD信息: 
```powershell
mfc0    $t0, $13        # load cause-register
srl     $s0, $t0, 2     # right-shift
andi    $s0, $s0, 0x1f  # mask => $s0 is EXCCode
slt     $s1, $t0, $zero # if $t0 < $zero then cause[31] is 1'b1 => BD = 1
```

##### EPC寄存器

当检测到异常指令时, EPC寄存器将发生异常的指令所处地址存入其中. 若发生异常的指令位于延迟槽, 应当把上一指令(跳转指令)的地址存入: 
```verilog
reg [31:2] _Epc; // Read & Write

always @(posedge Clk) begin
    if (Rst) begin
        // reset EPC-Register
    end else begin
        if (WEn && Addr == `EPC_Addr) begin
            _Epc <= Value[31:2];
        end else if (Irq) begin
            _Epc <= BD ? (PC[31:2] - 30'b1) : PC[31:2];
        end
    end
end

assign EPC = {_Epc, 2'b0}; // support eret

assign RData = (Addr == `EPC_Addr) ? _Epc : // ...
```

##### CP0插入流水线与异常码流水

考察异常码表, 不难发现可能发生异常的流水级为 F, D, E, M级, 因此我们理应**把协处理器0插入到M级**, 用于接受异常信息. 考虑到这四个流水级可能同时发生异常, 因此我们须把异常码流水. 并约定: **若当前级有异常, 前级也传来异常, 则向下一级传递当前级异常**.

![]({{ '/img/P-CPU-EXC.svg' | prepend: site.baseurl}})

> 我们称M级为宏观级. F, D, E级的指令"未曾执行", M级指令“正在执行”, W级指令“执行完毕”. 相当于把流水线CPU封装为一个单周期 CPU. 

#### 进入与退出异常处理程序

##### 进入异常处理程序

当检测到异常时, CP0的CAUSE寄存器存入异常码与BD信息, EPC存入回滚地址, SR的EXL置1; 同时, 流水线寄存器全部清空, PC置为异常处理程序入口`0x00004180`.

原则上, 检测到异常时, 我们要求已经"执行完毕"的指令继续执行完, "未曾执行"与"正在执行"的指令被撤回. 但由于乘除法元件相关指令在E级生效, 因此本设计允许乘除法相关指令不撤回. 

##### 退出异常处理程序

当D级检测到`eret`指令时, 随即将IM的Addr端口改接入CP0的EPC, 并将PC的Next端口改接为EPC+4. 这样, 下一周期时, D级就是用户态指令, `eret`后的指令被屏蔽:

```verilog
/* IM.Addr = */ instrD_eret ? CP0.EPC : PC.PC;
/* PC.Next = */ instrD_eret ? (CP0.EPC + 4) : /* Next-PC */;
```

值得注意的是, `eret`处于D级时, 仍然是"未曾执行"的, 因此, 只有当`eret`流水至M级时, 才把SR寄存器的EXL复位.

```verilog
/* CP0.EXL_Rst = */ instrM_eret;
```

另外, 需注意`mtc0`有可能写EPC, 因此要特判`mtc0-eret`的指令组合, 予以阻塞或转发机制.

## 外设与中断

外设是计算机系统的重要组成部分, 如显示器, 键盘, 鼠标等. CPU与外设交互的方式主要有两种:
1. 轮询
2. 中断

轮询方式就是在一循环内不断读取外设的信息, 从而响应外设的信号改变. 轮询的方式不利于极高频率的CPU与极低频率的外设相匹配.

我们的系统采用的方式是中断. 当外设发出中断信号时, CPU进入**中断处理程序**, 在中断处理程序中与外设交互. 

为实现上的简化, 中断处理程序与异常处理程序为同一程序, 入口地址均为`0x00004180`; 另外, 我们认为中断信号发出后, 会持续数个周期(5到10周期).

### CP0支持中断

根据引言, 中断对应的异常码为`0`.

本设计中将中断信号直接连入M级. 由于中断是不可预测地由外设引发, 而异常是蕴含于指令中, 因此当异常和中断同时出现在M级时, 应优先响应中断.

#### SR寄存器 - IM, IE & CAUSE寄存器 - IP

SR寄存器中IM段用于屏蔽某些外设的中断信号, 而IE段用于指示是否响应中断.

CAUSE寄存器中IP段用于每周期无条件地存入外设中断信号.

```verilog
//...
always @(posedge Clk) begin // CAUSE-Register
    if (Rst) begin
        // reset CAUSE-Register
    end else begin
        if (Irq) begin
            // ...
            _Cause[6:2] <= HardwareIntRq ? 0 : EXCCode;
        end
        _Cause[15:10] <= HwIrq; // Interrupt_Pend
    end
end
// ...
wire HardwareIntRq = (|(HwIrq & _Sr[15:10]) & _Sr[0] & ~_Sr[1]);
                   // Not masked            & IE     & !EXL
wire ExceptionIntRq = |EXCCode; // EXCCode != 5'b0
assign Irq = HardwareIntRq | ExceptionIrq;
```

### 气泡中断

流水线在某些情况下阻塞产生气泡`nop`, 流水线寄存器中的PC值与BD值无效, 若这些气泡到达M级时, 外部传来中断, 则传入CP0的PC值与BD值必须作一些特殊处理: **向前级找非气泡指令, 以非气泡指令的PC与BD传入CP0**. 非气泡指令的特征是: PC在IM范围内**或**对应的EXCCode非0.

```verilog
/* CP0.PC = */ isLegalPC(instrM_PC) || (|instrM_EXCCode) ? instrM_PC :
               isLegalPC(instrE_PC) || (|instrE_EXCCode) ? instrE_PC :
               isLegalPC(instrD_PC) || (|instrD_EXCCode) ? instrD_PC : 32'b0;
/* CP0.BD = */ isLegalPC(instrM_PC) || (|instrM_EXCCode) ? instrM_BD :
               isLegalPC(instrE_PC) || (|instrE_EXCCode) ? instrE_BD :
               isLegalPC(instrD_PC) || (|instrD_EXCCode) ? instrD_BD : 1'b0;
```

> 把连入CP0的PC称作**宏观PC**, 连入CP0的BD称为**宏观BD**.

### 定时器 - 模拟外设I/O与中断

定时器是倒计时的设备, 可以通过`lw`, `sw`指令读写定时器. 本设计中两个定时器的规格相同, 如下所示:

寄存器 | 读写 | 说明
:-:|:-:|:--
控制寄存器`ctrl` | R/W | 具体用途见[说明](#控制寄存器)
初值寄存器`init` | R/W | 开始计数时, 其值加载到计数值寄存器中
计数值寄存器`count` | R | 倒计数

#### 控制寄存器说明

`ctrl` | 名称 | 功能
:-:|:-:|:--
`ctrl[0]` | 计数使能 | 当计数使能为1时, 倒计时开始运行
`ctrl[2:1]` | [模式](#模式说明) | 当`ctrl[2:1]==2'b0`时为模式0, 否则为模式1
`ctrl[3]` | 中断使能 | 只有当中断使能为1时, 倒计时到0会产生中断信号

#### 模式说明

定时器有两个模式. 无论哪个模式, `sw`写寄存器总是优先于下面的状态变化.

##### 模式0

当计数值寄存器倒计时为0时, 停止计数. 此时控制寄存器使能变为0, 若中断使能有效, 则中断信号持续输出. 直至控制器寄存器使能置1, 初值寄存器值载入计数值寄存器, 中断信号停止输出, 计数值寄存器重新倒计数.

##### 模式1

当计数值寄存器倒计时为0时, 若中断使能有效, 则中断信号输出一周期. 一周期后, 初值寄存器值载入计数值寄存器, 重新开始倒计数.

#### 操作规范

1. 只有控制寄存器和初值寄存器可写, `sw`写计数值寄存器将引发`AdES`异常
2. `sw`对控制寄存器和初值寄存器的写操作优先于状态转移
3. 计数前应置`ctrl[0]`为0, 加载初值寄存器后再启用计数
4. 如果不需要产生中断, 则应该屏蔽中断(`ctrl[3]`)

### 系统桥

本设计中外设与数据存储器均连入M级, 需要使用系统桥进行线路选择. 系统桥一端直接接受CPU中M级传来的地址, 写使能, 写数据, 并向CPU发送读出的数据; 另一端向外设与数据存储器发送地址与写使能, 写数据, 并从外设与数据存储器接受读出的数据. 以**CPU-系统桥-DM**线路为例:

```verilog
/* DM.WEn = */ hitDM(CPU.Addr) ? CPU.WEn : 1'b0;
/* DM.Addr = */ CPU.Addr;
/* DM.WData = */ CPU.WData;

/* CPU.RData = */ hitDM(CPU.Addr) ? DM.RData : //...
```

其中:

```verilog
function hitDM;
input [31:0] addr;
begin
    hitDM = (addr >= `DM_Base && addr <= `DM_Ceil);
end
endfunction
```

> 此外, 系统桥还担任M级的异常产生部件, 通过内部的组合逻辑, 即可判定地址是否异常.

## 微系统全貌

![]({{ '/img/MIPS-MicroSys.svg' | prepend: site.baseurl}})

## 微系统测试

### 正常程序测试

首先需要进行所有指令的正确性与冲突性测试, 其方法可参考流水线CPU的测试方案.

### I/O测试

使用`lw`与`sw`与定时器进行交互. 为使用MARS对拍, 可以对MARS源码进行修改, 可以参考[项目](https://github.com/Chenrt-ggx/ComputerOrganization).

### 异常/中断测试

#### 异常/中断处理程序的编写

编写异常/中断处理程序的主要步骤为:
1. 保存现场: 寄存器存入栈
2. 分析异常码: 与CP0交互, 通过掩码, 移位等操作获得异常码. 随后进行异常码分析, 调用不同的处理函数
3. 恢复现场: 寄存器从栈中取出
4. 返回用户代码段: `eret`

#### 异常处理测试

可以使用python编写一个辅助程序, 生成具有异常代码的汇编程序. 使用ISim与MARS对拍即可完成测试. 注意, 每一个异常产生原因都要专门测试. 为使MARS产生某些特定异常(如: 写定时器的只读寄存器), 可以对MARS进行修改, 可以参考[项目](https://github.com/Chenrt-ggx/ComputerOrganization).


#### 中断处理测试

一方面, 可以使用定时器来产生周期性的中断信号: 把定时器1的中断信号传入CP0的`HwIrq[0]`, 把定时器2的中断信号传入CP0的`HwIrq[1]`. 在用户程序开始时, 将定时器初值设为周期, 随后启动定时器. 定时器的模式0可用于产生长时间中断, 模式1可用于产生周期性中断.

另一方面, 为使用测试平台Testbench进行**针对某些PC**(称需要针对的PC为**特征PC**)的中断测试, 可以把宏观PC连接到顶层模块, 并把测试平台的中断信号连入CP0的`HwIrq[2]`. 在Testbench中, 每个上升沿检测宏观PC是否等于特征PC, 若相等:
1. 立即向CP0发送若干周期的中断信号.
2. 延迟一段时间后, 再向CP0发送若干周期的中断信号.

由于MARS不支持外部中断, 因此若要检验中断的实现是否正确, 一种方法是通过肉眼调试的方法进行检验, 另一方法是与已被检验的微系统进行对拍验证.

## 微系统增量开发

* **增加设备**: 只需对系统桥进行增量开发即可(注意扩展内存地址), 随后进行I/O与中断测试.
* **增加异常类型**: 只需对异常检测进行增量开发即可, 随后进行异常处理测试.

