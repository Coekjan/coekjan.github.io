---	
layout:     post	
title:      『Computer Architecture』 Pipeline CPU	
subtitle:   『计算机体系结构』 流水线CPU    
date:       2021-01-21	   
author:     Coekjan 
header-img: img/post-bg-CA.jpg	
catalog:    true    
katex:  true    
tags:	
    - Computer Architecture  
---

本节将构造一个支持引言指令集中前50条指令的流水线CPU. 暂不进行异常检测.

## 流水线思想

### 单周期缺陷

单周期CPU使用一周期完成一条指令, 包括 **取指令(F)->译码(D)->执行(E)->访存(M)->写回(W)** 五个步骤, 这意味着无论CPU当前运行的指令是否完全需要这五个步骤, 都要经历一整个周期的时间才能取出下一指令.

例如: `addu`指令并不需要访存(M), `j`指令并不需要执行(E)...

这将导致单周期CPU的重大缺陷: CPU时钟周期必须与指令集中**执行时间最长**的指令相匹配. 这条执行时间最长的指令即为**关键路径**.

> 例如, `lw` 需要完全经历上述五个步骤才能完成, 因此CPU的时钟周期必须要比 `lw` 的执行时间要长.

![]({{ '/img/SCPU-Disadvan.svg' | prepend: site.baseurl}})

### 五级流水线

> 流水线是一种实现指令级并行的技术手段.

工厂生产零件应用了流水线的思想: 将工人分为三个组别A, B, C. A组对零件进行初加工, B组接受A组加工结果进行进一步加工, C组接受B组加工结果进行最终加工. 这样安排的好处显而易见: 允许有三个零件同时在生产线中生产, 每一个组别的工人都完全投入工作, 没有工人空闲.

同样的原理可应用于CPU技术中. 我们在上面的讨论中得知, 指令执行包含 **取指令(F)->译码(D)->执行(E)->访存(M)->写回(W)** 五个阶段.

> 以下的流水线均指五级流水线.

下图比较了同一指令序列在单周期与流水线上的运行效果.

![]({{ '/img/P-CPU-Time.svg' | prepend: site.baseurl}})

可以看见流水线的本质是指令的并行执行. 为实现流水线, 我们须在单周期CPU的通路中增加流水线寄存器, 用以保存上一阶段的结果, 并为当前阶段提供数据. 下图是在上一节中单周期通路插入了流水线寄存器.

![]({{ '/img/P-CPU-DP-1.svg' | prepend: site.baseurl}})

> 译码 (D) 级虽称为"译码", 但实际上进行了寄存器的读取, 立即数的扩展等.

本系列博文约定:
* F与D级之间的流水线寄存器称为**D级寄存器**
* D与E级之间的流水线寄存器称为**E级寄存器**
* E与M级之间的流水线寄存器称为**M级寄存器**
* M与W级之间的流水线寄存器称为**W级寄存器**

## 流水线冒险 - Hazard

### 结构冒险, 数据冒险与控制冒险

**结构冒险**: 因缺乏硬件支持而导致指令不能在预定的时钟周期内执行. 若IM与DM使用同一个存储器实现, 则会导致不能同时取指令与取数据, 发生结构冒险. 我们的设计中不存在结构冒险.

**数据冒险**: 因指令间存在相关性, 无法提供指令执行所需的数据而导致指令不能在预定的时钟周期内执行. 如:

```powershell
addu    $1, $2, $3
subu    $2, $1, $3
```

在不做任何干预的情况下, 当`addu`仍处于流水线中时, `subu`无法从GRF中获取正确的`$1`的值. 我们采用转发与阻塞的方法解决这种冒险.

**控制冒险**: 由于分支, 取得的指令并不是所需运行的指令而导致指令不能在预定的时钟周期内执行. 我们的设计中使用延迟槽解决这种冒险.

### 转发与阻塞

**需求-供给模型**: 当某一端口需要使用数据时, 我们称之为**需求端**, 为之提供数据的端口称为**供给端**. 我们只需要二把某一需求端对应的所有供给端找到, 并置一多路选择器进行选择供给的数据, 通过控制器得到合适的选择信号, 需求端即可得到正确的数据.

> 需要注意的是我们的设计中供给端必须是**寄存器**的输出端, 否则将有可能使得关键路径延长, 时钟频率受到限制.

以ALU的SrcA端口为例, SrcA是一需求端, 其供给端有可能是E级流水寄存器, M级寄存器, W级寄存器. 显然**M级寄存器存有的数据是最新的, W级寄存器存有的数据是次新的, E级寄存器存有的数据是最旧的.** 举例如下:

考虑指令序列:

```powershell
addu    $1, $2, $3
addu    $1, $1, $3
subu    $2, $1, $2
addu    $2, $1, $1
```

![]({{ '/img/P-CPU-ALUSrcA-forwrarding.svg' | prepend: site.baseurl}})

但有些情况下仅靠转发不能完全解决数据冲突, 如:

```powershell
lw      $1, 0($0)
addu    $2, $1, $3
```

当`addu`到达E级, ALU的SrcA端是需求端, 但是`lw`还处于M级, 未取出`$1`的数据. 此时只能暂停流水线一周期, 即PC和D级流水线寄存器的使能端置`0`, E级流水线寄存器插入"气泡"`nop`. 一周期后, `lw`产生的数据就可以通过转发传递给`addu`了.

> 后文 **AT 方法**将详细说明转发与阻塞的判定与信号的产生.

![]({{ '/img/P-CPU-ALUSrcA-stall.svg' | prepend: site.baseurl}})

### 跳转延迟槽

本设计中, 我们在D级加入一个比较模块CMP, 用于分支指令的条件判断, 并允许延迟槽: 若`PC`指向分支指令, 那么执行完分支指令后, 将继续执行`PC+4`对应的指令(称这个指令位于**延迟槽**内), 随后再执行分支后的指令. 这样的设计可以有效解决控制冒险 - 分支指令有足够的时间用于判断转移与否.

> 编译器将调度合适的指令插入延迟槽.

## 流水线数据通路

### 指令集分析

分析[指令集](https://blog.coekjan.cn/2021/01/13/Introduction/#指令集仅支持定点指令)中前50条指令, 其过程与单周期中的分析表格类似. 我们有以下结论:
1. GRF需要支持内部转发功能.
2. ALU需要支持的运算有加法, 减法, 逻辑左移, 逻辑右移, 算术右移, 按位与, 按位或, 按位异或, 按位或非, 高位加载, 小于置位.
3. 需支持字节, 半字, 整字三种写内存方式, 为此, 引入新模块BE产生字节使能, DM需支持字节使能端口.
4. 需支持字节, 半字, 整字三种读内存方式, 为此, 引入新模块DBS, 对DM的输出进行截选.
5. 需支持乘除法与`HI`, `LO`两个寄存器, 应在E级增加一乘除法元件MDU.
6. 由于D级与F级间有一个周期延迟, 需另加一`+4`模块完成PC计算, NPC仅完成转移型指令的PC计算.

#### GRF的内部转发

以`RData1`为例:

```verilog
assign RData1 = (Addr1 == 5'b0)         ? 32'b0 :
                (Addr1 == Addr3) && WEn ? WData : reg_array[Addr1]; // 内部转发
/*
 * 不支持内部转发时:
 * assign RData1 = (Addr1 == 5'b0) ? 32'b0 : reg_array[Addr1];
 */
```

其本质是W级向D级的转发过程.

#### 字节使能的产生与使用, 数据截选

##### BE模块

![]({{ '/img/CPU-BE.svg' | prepend: site.baseurl}})

```verilog
module BE(
    input [1:0] Addr10,
    input [`StTypeWidth - 1:0] StType,
    output [3:0] ByteEn
);
    assign ByteEn = StType == `StWord ? (4'b1111) :
                    StType == `StHalf ? (
                        Addr10 == 2'b00 ? 4'b0011 :
                        Addr10 == 2'b10 ? 4'b1100 : 4'bx
                    ) : 
                    StType == `StByte ? (
                        Addr10 == 2'b00 ? 4'b0001 :
                        Addr10 == 2'b01 ? 4'b0010 :
                        Addr10 == 2'b10 ? 4'b0100 :
                        Addr10 == 2'b11 ? 4'b1000 : 4'bx
                    ) : 4'bx;
endmodule
/*
 * 对于一个整字, 可以划分为四个字节, 若某字节对应的使能为1, 则将其写入, 否则不写入.
 */
```

##### DM模块

![]({{ '/img/CPU-DM-BE.svg' | prepend: site.baseurl}})

```verilog
/*
 * input [31:0] Addr
 * input [31:0] WData
 * input [3:0] ByteEn
 * reg [31:0] data_mem [0:`DataMemSize - 1];
 */
wire [31:0] odata   = data_mem[Addr[/* 截选地址来寻址 */]];
wire [31:0] wr_data = ByteEn == 4'b1111 ? WData :
                      ByteEn == 4'b0011 ? {odata[31:16], WData[15: 0]}               :
                      ByteEn == 4'b1100 ? {WData[15: 0], odata[15: 0]}               :
                      ByteEn == 4'b0001 ? {odata[31: 8], WData[ 7: 0]}               :
                      ByteEn == 4'b0010 ? {odata[31:16], WData[ 7: 0], odata[ 7: 0]} :
                      ByteEn == 4'b0100 ? {odata[31:24], WData[ 7: 0], odata[15: 0]} :
                      ByteEn == 4'b1000 ? {WData[ 7: 0], odata[23: 0]}               : 32'bx;
// wr_data 是最终要写入的整字
```

##### DBS模块

![]({{ '/img/CPU-DBS.svg' | prepend: site.baseurl}})

```verilog
module DBS(
    input [1:0] Addr10,
    input [31:0] RData,
    input [`LdTypeWidth - 1:0] LdType,
    output [31:0] Data
);
    assign Data = LdType == `LdWord      ? RData :
                  LdType == `LdHalfUnsgn ? (
                      Addr10 == 2'b00 ? {16'b0, RData[15: 0]} :
                      Addr10 == 2'b10 ? {16'b0, RData[31:16]} : 32'bx
                  ) :
                  LdType == `LdHalfSgn   ? (
                      Addr10 == 2'b00 ? {{16{RData[15]}}, RData[15: 0]} :
                      Addr10 == 2'b10 ? {{16{RData[31]}}, RData[31:16]} : 32'bx
                  ) : // ...
endmodule
```

#### 模拟乘除法元件MDU

本设计中约定, 乘除法使用有限状态机进行模拟仿真:
1. 乘法耗时5周期, 除法耗时10周期;
2. 乘除法元件接受到Start信号后的第一个时钟上升沿开始执行运算, 输出Busy为1;
3. 运算结果保存在`HI`和`LO`后, Busy置0;
4. 当Start或Busy为1时, 其他涉及乘除法元件的指令均被阻塞在D级.
5. 数据写入`HI`或`LO`只需一周期.

![]({{ '/img/CPU-MDU.svg' | prepend: site.baseurl}})

其端口功能如下:

端口 | 方向 | 说明
:-: | :-: | :--
Clk | I | 时钟信号
Rst | I | 同步复位信号
HIWEn | I | HI寄存器写使能
LOWEn | I | LO寄存器写使能
Value[31:0] | I | 时钟上升沿来临时, 若复位信号为0, 相应写使能有效, 则向相应寄存器写入Value
Data1[31:0] | I | 运算数1
Data2[31:0] | I | 运算数2
Ctrl[`MDUCtrlWidth - 1:0] | I | 控制信号
Start | I | 启动信号
Busy | O | 指示模块是否忙
HI[31:0] | O | HI寄存器的值
LO[31:0] | O | LO寄存器的值

### 构建无旁路的通路 - 功能实现

通过指令集分析, 我们可以构造出如下的通路, 此通路并未考虑转发旁路:

![]({{ '/img/P-CPU-DP-2.svg' | prepend: site.baseurl}})

### 增加转发旁路 - 冲突解决

应用上文的需求-供给模型, 我们不难找到所有的转发旁路:
1. D级需求: E->D(如序列`jal-addu`), M->D(如序列`jal-nop-addu`)(W->D隐藏于GRF的内部转发中);
2. E级需求: M->E(如序列`addu-addu`), W->E(如序列`addu-nop-addu`)
3. M级需求: W->M(如序列`addu-sw`)

那么只需在需求端给予多选器, 接受供给端的数据, 即可完成旁路的设计.

![]({{ '/img/P-CPU-DP-3.svg' | prepend: site.baseurl}})

## 流水线控制

在流水线控制中, 有两种主要的译码方法:
1. 集中式译码: 在D级完成所有控制信号的译码, 并把将要用到的所有信号参与流水.
2. 分布式译码: 将指令流水, 在每一级都进行译码, 只取出当前级所需的控制信号.

集中式译码更节省组合逻辑, 但实现起来较为麻烦; 分布式译码更加容易实现, 但对资源浪费较为严重.

### 功能性信号产生

功能性信号产生的方法与单周期的控制相似, 主要的步骤就是识别指令与生成信号. 值得注意的是, 回写寄存器时, 我们在流水线中有一个特殊的处理:

```verilog
assign GRF_WE = 1; // 写使能恒为1
assign GRF_Addr3 = /* write GRF[rt] */ ? rt    :
                   /* write GRF[rd] */ ? rd    :
                   /* write GRF[31] */ ? 5'd31 : 0; // 若不写寄存器, 则置Addr3为0
```

> 写 `0` 寄存器相当于不写寄存器.

这样处理的优势在下文所述的AT方法中得到体现.

### 转发与阻塞信号产生 - AT方法

指令**A信号**: 该指令将写往的寄存器编号, 若不写寄存器, 则编号为0.

指令**T信号**:
* $\rm T_{new}$ : 从E级开始计算, 指令产生结果需要的时钟周期.
* $\rm T_{use}$ : 从D级开始计算, 指令必须使用寄存器值的时钟周期.

显然, 一个具体的指令, 只能有一个A信号, 一个 $\rm T_{new} $信号, 最多两个 $\rm T_{use}$ 信号(`rs`与`rt`).

以指令集{`addu`, `ori`, `beq`, `lw`, `sw`, `jal`}为例:

指令 | A信号 | $\rm T_{new}$ | $\rm T_{use} \sim rs$ | $\rm T_{use} \sim rt$
:-: | :-: | :-: | :-: | :-:
`addu` | `rd` | 1 | 1 | 1
`ori` | `rt` | 1 | 1 | $\infty$
`beq` | `0` | 0 | 0 | 0
`lw` | `rt` | 2 | 1 | $\infty$
`sw` | `0` | 0 | 1 | 2
`jal` | `31` | 0 | $\infty$ | $\infty$

> $\infty$ 表示该指令不会用到对应的寄存器值, 实际编码过程中可取流水线的级数.

通过上述分析, 可以轻松得到转发的控制方法. 以D级为例:

```verilog
// E->D, M->D
assign MUXSel_rs_D = (InstrE_A != 5'b0 && InstrD_rs == InstrE_A) ? `EtoD :
                     (InstrM_A != 5'b0 && InstrD_rs == InstrE_A) ? `MtoD : `NoForwarding;
assign MUSSel_rt_D = (InstrE_A != 5'b0 && InstrD_rt == InstrE_A) ? `EtoD :
                     (InstrM_A != 5'b0 && InstrD_rt == InstrE_A) ? `MtoD : `NoForwarding;
// ...
```

> 上述写法中体现了转发的优先级.

也可得到阻塞的控制方法. 以`rs`的阻塞为例:

```verilog
wire Stall_rs_E = (InstrE_A != 5'b0 && InstrD_rs == InstrE_A) && (InstrE_Tnew > InstrD_Tuse);
wire Stall_rs_M = (InstrM_A != 5'b0 && InstrD_rs == InstrM_A) && (InstrM_Tnew > InstrD_Tuse);

wire Stall_rs = Stall_rs_E || Stall_rs_M;
//...
```

> 特判A信号为 `0` 是因为 `0` 寄存器值恒为 `0` , 不可以接受转发数据.

此外, 乘除法元件的阻塞行为需要另行判断:

```verilog
wire Stall_MDU = (MDU_Start || MDU_Busy) && InstrD_NeedMDU;
```

## 流水线组装

流水线的组装与单周期组装相似, 此处不再详述.

## 流水线系统测试

由于我们实现的指令集规模较大, 因此必须首先进行正确性测试, 保证指令功能正常. 正确性的测试方法就是在指令序列中插入足够的`nop`, 避免冲突产生, 仅仅对指令的行为进行测试.

流水线最为"凶险"的领域就是冒险解决. 在正确性测试后, 我们应着力进行数据冲突与控制冲突的序列构造.

数据冲突的测试: 假设我们的指令共有 $n$ 种 $\rm T_{new}$ , $m_1$ 种 $\rm T_{use}\sim rs$ , $m_2$ 种 $\rm T_{use}\sim rt$ . 那么我们可以得到最多 $n\cdot m_1\cdot m_2$ 种组合, 我们只需对这些组合进行逐一测试, 即可保证测试的完备性.

> 事实上, 我们50条指令中, $n=3,m_1=3,m_2=4$ , 不过 $36$ 种情形.

控制冲突的测试: 主要测试延迟槽行为的正确性, 着重测试延迟槽的转发, 延迟槽的阻塞.

## 流水线增量开发

使用AT方法有助于流水线的增量开发:
1. RTL分析: 分析新指令RTL, 若已有部件可以支持新指令, 则直接进行指令集分析; 否则则需要为其增加必要的新部件;
2. 指令集分析: 在已有的指令分析表格中增加待添加的指令, 并重新合并分析输入端的数据来源;
3. 通路增量开发: 根据指令集分析, 增加必要的MUX;
4. AT信息译码: 根据RTL, 可以得到指令的AT信息;
5. 功能信号增量开发: 根据指令集分析, 增加必要的控制信号;
6. 针对新指令进行测试.

> 对于特殊指令, 建议采取特殊对待的方法进行开发.