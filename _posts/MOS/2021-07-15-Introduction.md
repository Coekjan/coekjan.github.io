---	
layout:     post	
title:      『MOS』 Introduction	
subtitle:   『MOS』 引言    
date:       2021-07-15	   
author:     Coekjan 
header-img: img/post-bg-MOS.jpg	
catalog:    true	
katex:    true    
tags:	
    - MOS  
---

> 前置知识: [MIPS 计算机体系结构]({{ '/2-tags/#Computer%20Architecture' | prepend: site.baseurl}})

以下介绍均参考自[R30xx手册](https://cgi.cse.unsw.edu.au/~cs3231/doc/R3000.pdf). **阅读本文时, 请务必参考该手册**.

本系列文章中, 均**以中断代指外部中断, 以异常代指内部中断**.

## R3000地址空间

![]({{ '/img/OS-MIPS-Base-Address-Space.svg' | prepend: site.baseurl}})

## 软硬件接口(CP0)

> CP0, 即协处理器0, 为操作系统提供了软硬件接口.

CP0提供了系列寄存器, 用于记载CPU的运行信息. 指令集中 `mfc0` 和 `mtc0` 就是与CP0相关的重要指令.

ID | 寄存器名 | 功能 | 备注
:-: | :-: | :-- | :--
15 | PRId | 存储CP0类型等信息 | 无需过多关注
12 | SR | 状态寄存器, 存储CPU的运行模式 | 重点关注其中的 `IM` 与 `KU`, `IE` 段
13 | Cause | 当中断异常发生时, 该寄存器记录导致中断异常发生的原因 | 重点关注其中的 `BD`, `IP`, `ExcCode` 段
14 | EPC | 当中断异常发生时, 该寄存器记录当前的PC寄存器值 | 注意延迟槽内发生中断异常时, EPC记录延迟槽上一指令的地址
8 | BadVaddr | 当发生TLB异常, 或用户程序试图访问kuseg之外的地址, 或地址与其引用的数据大小错误对齐时, 该寄存器保存这个引发异常的地址 | MOS中, 通过此寄存器获取引发TLB缺失的地址
10 | EntryHi | 与EntryLo一起工作, 所有读写TLB的操作都要通过这两个寄存器 | 重点关注其与TLB的协作关系, 以及中断异常发生时该寄存器的行为
2 | EntryLo | | 重点关注其与TLB的协作关系
0 | Index | TLB读写的相关指令需要用到该索引寄存器
1 | Random | 随机填写TLB表项时需要用到该随机寄存器 | Random寄存器的本质是不断运行的计时器

下面先重点介绍几个寄存器.

### SR中的 `IM` 与 `KU`, `IE`

![]({{ '/img/MIPS-CP0-SR.svg' | prepend: site.baseurl}})

> * `IM`: Interrupt Mask
> * `KU`: Kernel-mode or User-mode
> * `IE`: Interrupt Enable

`IM` 段共8位, 当第i位为1时, 意味着当第i中断信号置高时, CPU应响应中断.

![]({{ '/img/MIPS-CP0-SR-IM.svg' | prepend: site.baseurl}})

`KU`, `IE` 共有6个位, 组成了一个深度为3, 宽度为2的栈. 关于这一点, 初次接触MOS的读者可无需理会这一结构, **只需要关注其中最低的两位**:
1. `KUc` 为1, 意味CPU目前在内核态下运行; `KUc` 为0, 意味CPU目前在用户态下运行.
2. `IEc` 为1, 意味着CPU会响应中断; `IEc` 为0, 意味着CPU不会响应中断.

### Cause中的 `BD`, `IP`, `ExcCode`

![]({{ '/img/MIPS-CP0-Cause.svg' | prepend: site.baseurl}})

> * `BD`: Branch Delay
> * `IP`: Interrupt Pending
> * `ExcCode`: Exception Code

`BD` 的引入, 是为了处理MIPS流水线延迟槽下发生异常中断的情形.

> 延迟槽: MIPS 流水线中, **跳转指令的下一指令执行完毕后才发生跳转**. 如:
> 
> ```powershell
> jal       tag                 # <~~ 1
> addiu     $31, $31, 4         # <~~ 2 delay-slot
> sw        $31, 0($29)
> addiu     $29, $29, -4
> tag:
> lw        $31, 0($29)         # <~~ 3
> addiu     $29, $29, 4         # <~~ 4
> ```
> 
> 引入延迟槽, 是为了解决流水线的控制冒险, 属于 MIPS 流水线的特性. 
> 
> **考虑执行延迟槽指令时的异常与中断行为**. 若 EPC 保存发生异常中断时的指令地址, 即延迟槽指令的地址, 那么当 CPU 回滚用户态时, 将从延迟槽指令开始往后顺序执行, 而没有执行跳转行为. **针对这一特殊情况, EPC 会保存延迟槽的上一条指令的地址**, 即跳转指令的地址, 使得回滚用户态时, CPU 重新执行跳转指令, 保持了执行顺序的正确性. **为了提示软件此时 EPC 中存储的并不是真正的中断异常地址, 就引入了 `BD` 位**.

当 `BD` 位被置高时, 就意味着 EPC 中存储的是**发生中断异常的指令的上一指令的地址**.

`IP` 用于记录当前发生的中断信息, 它并不指示异常发生时发生了什么, 而是指示当前正在发生什么. MOS中, 就是通过对 `IP` 的判定来识别时钟中断的.

`ExcCode` 用于记录异常信息, MOS中需要重点关注下列异常码:

异常码 | 异常名缩写 | 描述
:-: | :-: | :--
0 | Int | 外部中断
1 | Mod | TLB被修改
2 | TLBL | TLB load异常
3 | TLBS | TLB store异常
8 | Syscall | 系统调用, 仅由 `syscall` 指令引发

本文的[响应中断异常的基本框架](#响应中断异常的基本框架)将告诉读者, MOS如何识别不同的异常码, 从而进行不同的处理.

### EntryHi与EntryLo

EntryHi与EntryLo的位结构如下:

![]({{ '/img/MIPS-CP0-EntryHi&Lo.svg' | prepend: site.baseurl}})

* Key:
  * VPN: Virtual Page Number.
    * 作用一: 当TLB缺失时, **EntryHi中的该段**将自动设置为引发TLB缺失的虚地址. **MOS中, 这一特性在TLB重填时起到关键作用**.
    * 作用二: 当需要填写TLB表项, 或进行TLB检索时, 软件应根据需要设置**EntryHi中的该段**为对应的虚地址.
  * ASID: Address Space Identifier.
    * 用于区分不同的地址空间, **使用虚地址查找对应的实地址时, 就需要将虚地址与对应的ASID一同作为Key来查找TLB表项**. MOS中不同的进程对应着不同的ASID, 从而使得**不同进程的具有不同的地址空间**.
* Data:
  * PFN: Physical Frame Number.
    * 软件**通过 `mtc0` 写入EntryLo中的该段**, **随后使用 `tlbwr` 等写TLB指令**, 才将此时的EntryHi与EntryLo写入到TLB表项中.
  * N: Non-cacheable. 当该位被置高时, 后续访问物理内存将不通过cache.
  * D: Dirty. 事实上是可写位. **当该位被置高时, 该地址可写; 否则任何写操作都将引发TLB异常**.
  * V: Valid. **如果该位为0, 则任何访问该地址的操作都将引发TLB缺失**.
  * G: Global.

## 虚实地址转换机构(TLB)

### 通过TLB访存

R3000中, 访问kuseg和kseg2这两个内存区域需要通过TLB映射到具体的物理页面.

在MOS中, 我们采用的R3000芯片为Basic版本. 该版本的R3000芯片没有MMU, **唯一的虚实地址转换机构是TLB**. 访问地址的大致步骤如下, **这些步骤都是硬件实现的**:
1. CPU生成虚地址(取指令, 存取内存) $\mathtt{Addr_{31...0}}$ : 首先将虚地址的低12位 $\mathtt{Addr_{11...0}}$ 抹去, 然后将得到的 $\mathtt{Addr_{31...12}\mid0^{12}}$ 作为**VPN, 与CP0的EntryHi寄存器中ASID段一同, 作为键Key来访问TLB**.
2. 以上一步中的Key作为键访问TLB: 若找到了对应的值, 即找到了对应的PFN, 则将PFN与 $\mathtt{Addr_{11...0}}$ 进行拼接, 组成实地址.
3. 判定操作的有效性. 若TLB项中, **V为0**, 或**当前为写操作且D为0**, CPU将产生TLB异常, 同时**BadVaddr将存入此时的虚地址, EntryHi等寄存器将填充相关信息(EntryHi的VPN填充为此时的虚地址)**, 异常处理软件可通过这些寄存器获取对应的异常信息.
4. 根据TLB项中的Cache位, 进行后续的访存.

由于R3000(Basic)不具有硬件MMU, 因此当TLB缺失时, 就需要软件手段来填写TLB表项, 这一内容将在后续的文章中提及.

### TLB相关指令

* `tlbr`: 根据Index索引寄存器, 读出TLB中对应的表项到EntryHi与EntryLo中.
* `tlbwi`: 根据Index索引寄存器, 将EntryHi与EntryLo的数据写入到TLB中对应的表项.
* `tlbwr`: 将EntryHi与EntryLo的数据**随机写入到一个TLB表项中**.
* `tlbp`: 根据EntryHi中的VPN与ASID查找与之匹配的TLB表项, **将查找到的TLB表项的索引存入Index寄存器**(**若未查找到匹配项, 则Index的最高位为1, 即为负数**).

这些指令在实现内存管理时需要用到.

### TLB编程

TLB编程的软件方案总是如下所示的:

$$
\begin{CD}
    \mathtt{(Maybe~~Automatically)~Write~~EntryHi/Lo...}\\
    @VVV\\
    \mathtt{Call~~TLB-instructions~(tlbwi/tlbwr...)}
\end{CD}
$$

举个例子. 假设TLB缺失, 则CPU将处理该异常(*至于如何转入到下面的代码, 就涉及中断异常框架了, 此处略去*). 由于TLB缺失时, 硬件会**自动将引发TLB缺失的虚地址存入BadVaddr和EntryHi的VPN段**, 故可以进行一系列分析(*MOS系统中采用二级页表, 此处先不详细说明*). 分析后, 我们得知该虚地址应映射到某一实地址(4KB对齐), 假设此时**该实地址的高20位存在 `k1` 寄存器, 且寄存器的低12位已经设置为合理的权限位**, **则可以通过下面的代码将该实地址存入到TLB中**.

```powershell
mtc0    k1, $2      # $2 <==> EntryLo
tlbwr
```

这样, `tlbwr` 将此时EntryHi(*这里EntryHi已经被硬件自动设置为虚地址了*)与EntryLo的值随机写入到TLB中的一个条目.

> 真正的TLB填写将比上述过程复杂得多, 这将会在后续的文章中详细介绍.

## 响应中断异常的基本框架

### 中断异常入口

根据R3000手册, R3000的中断异常入口有5个, 此处列出其中在MOS中最重要的两个入口:

入口地址 | 所在内存区 | 描述
:-: | :-: | :--
`0x80000000` | kseg0 | TLB缺失时, PC转至此处
`0x80000080` | kseg0 | 对于除了TLB缺失之外的其他异常, PC转至此处

注意, 在MOS中, 仅仅实现了 `0x80000080` 处的中断异常处理程序. 这样也能工作的原因是: 当TLB缺失时, PC转至 `0x80000000` 后, **空转32周期(执行了32条 `nop` 指令)**, 到达 `0x80000080` , 随后也就与其他的中断异常一同处理了.

也就是说我们只需要实现 `0x80000080` 处的程序即可.

### 中断异常发生时CPU的行为

中断异常发生时, CPU将有以下行为:
1. **将当前PC值存入EPC寄存器**.
2. SR寄存器中, 将当前的IEc, KUc压入二重栈(这里压入二重栈的意思是: KUp和IEp写入到KUo和IEo; KUc和IEc写入到KUp和IEp), 并**将KUc置高, 标志着进入内核态**.
3. **Cause寄存器保存引发中断异常的原因**. **若是地址异常, 则BadVaddr和其他相关寄存器(如EntryHi)也被设置好**.
4. **将PC转至相应的入口**.

### 异常分发

内核启动时, 系统并不能够响应中断. 系统需要进行一系列的初始化工作后才能响应中断, 包括但不限于:
1. 内存管理初始化.
2. 进程管理初始化.
3. **异常分发**.
4. 时钟设置.

异常分发是将某一个异常码ExcCode绑定到某一个特定函数入口的过程. 在MOS中, 采用如下C代码完成异常分发:

> C 语言中, 直接使用函数名称, 本质上是进行了隐式类型转换, 转换成对应函数的入口地址.

```c
unsigned long exception_handlers[32];

void trap_init()
{
    int i;
    for (i = 0; i < 32; i++)
    {
        set_except_vector(i, handle_reserved);
    }
    set_except_vector(0, handle_int);
    set_except_vector(1, handle_mod);
    set_except_vector(2, handle_tlb);
    set_except_vector(3, handle_tlb);
    set_except_vector(8, handle_sys);
}
void *set_except_vector(int n, void *addr)
{
    unsigned long handler = (unsigned long)addr;
    unsigned long old_handler = exception_handlers[n];
    exception_handlers[n] = handler;
    return (void *)old_handler;
}
```

此处出现了许多函数名: `handle_reserved`, `handle_int`, `handle_tlb`, `handle_sys`. 但这些都不重要, **重要的是理解: 我们把这些函数的入口地址填入到了 `exception_handlers` 这个数组中去.** 这意味着, 当ExcCode为 `e` 时, 可以通过取 `exception_handlers[e]` 来得知该异常码对应的**中断异常处理子函数的入口**.

当异常分发完成后, 就可以启用全局中断(将SR寄存器中的IEc置高), 准备响应中断异常了. **问题是当中断异常到来时, 如何让PC转至相应的中断处理子函数**? 没错, 就是在 `0x80000080` 处写一段短小的引导程序即可!

```powershell
.section .text.exc_vec3         // load this section to 0x80000080
NESTED(except_vec3, 0, sp)
    .set    noat
    .set    noreorder
    mfc0    k1, CP0_CAUSE
    la      k0, exception_handlers
    andi    k1, k1, 0x7c
    addu    k0, k0, k1
    lw      k0, 0(k0)
    nop
    jr      k0
    nop
END(except_vec3)
```

> 值得注意的是, 这里完全使用 `k0` , `k1` 两个内核保留的寄存器, **可以保证用户态下其他通用寄存器的值不被改变**, 也就保持了现场不变.
> 
> 此外, 这里涉及到了两个 `.set` 指令:
> * `noat` : 意味着接下来的代码中**不允许汇编器使用 `at` 寄存器**. 这是因为此时刚刚陷入内核, 还未保存现场, 用户态下除了 `k0` , `k1` 之外都不能够被改变.
> * `noreorder`: 意味着接下来的代码中不允许汇编器重排指令顺序.

* `mfc0 k1, CP0_CAUSE`: 将Cause寄存器中的值取出, 存到 `k1` 寄存器.
* `la k0, exception_handlers`: `la` 为扩展指令, 意为load address. 该指令将上面提到的 `exception_handlers` 的**首地址**存到了 `k0` 寄存器.
  > `la` 指令本质是选用 `addiu` , `lui` , `ori` 等指令完成的扩展指令.
* `andi k1, k1, 0x7c`: 此处做位掩码, 使得 `k1` 中仅保留Cause寄存器中的ExcCode段.
  > 按位与 `0x7c` 即, 仅保留 6 - 2 位. 恰好是 ExcCode 在 Cause 寄存器中的位置.
* `addu k0, k0, k1`: 在前面的语句中, `k0` 寄存器保存了 `exception_handlers` 的首地址, `k1` 保存了**ExcCode乘4的值**, 此处做偏移, 使得 **`k0` 的值为 `exception_handlers` 数组的第ExcCode项**.
  > 数组中每一项占用 4 字节, 因此第 i 项的偏移量是 i×4 (即 i 左移 2 位).
  > 
  > 此处有一个巧合: `k1` 寄存器中, ExcCode 在 6 - 2 位, 这意味着 `k1` 本身就已经是 ExcCode 乘 4 的值了.
* `lw k0, 0(k0)`: 将此时 `k0` 指向的数组项取出, 仍存到 `k0` 中去. **执行完这一指令后, `k0` 的值就是数组第ExcCode项的值了, 也就是异常码ExcCode对应的中断异常处理子函数的入口地址**.
* `jr k0`: 跳转到该中断处理子函数.

至于中断处理子函数如何编写, 就是后续文章的内容了.