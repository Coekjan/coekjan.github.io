---	
layout:     post	
title:      『MOS』 Interrupt And Exception	
subtitle:   『MOS』 中断与异常    
date:       2021-08-26	   
author:     Coekjan 
header-img: img/post-bg-MOS.jpg	
catalog:    true	
katex:    true    
tags:	
    - MOS  
---

## R3000 中断异常的硬件行为

引言中提到了中断异常的硬件行为, 这里再重复说明. 这些行为将是理解中断异常软件处理程序的关键:
1. 将当前的**PC值存入CP0的EPC寄存器**(若PC此时为延迟槽, 则保存前一条指令的地址).
2. SR寄存器中, 将当前的IEc, KUc压入二重栈, 并**将IEc置为 `0`(关闭全局中断使能), 将KUc置高(进入内核态)**.
3. Cause寄存器中, **ExcCode段保存中断异常码**.
4. 若发生的是地址异常(这里特指TLB地址转换异常), 则**BadVaddr存入引发地址异常的地址**, **EntryHi的VPN段存入引发地址异常的虚页号**.
5. **PC转入中断异常入口**.

## MOS 中的中断异常处理方法

### 分发

PC转入中断异常入口后, 首先按照异常码找到中断异常处理函数入口:

```powershell
.section .text.exc_vec3
NESTED(except_vec3, 0, sp)
    .set noat 
    .set noreorder
    mfc0    k1, CP0_CAUSE
    la      k0, exception_handlers
    andi    k1, 0x7c
    addu    k0, k1
    lw      k0, (k0)
    nop
    jr      k0
    nop
END(except_vec3)
    .set at
```

这部分在引言部分有[逐行解释]({{ '/2021/07/15/Introduction/#%E5%BC%82%E5%B8%B8%E5%88%86%E5%8F%91' | prepend: site.baseurl}}).

MOS中的分发表如下:

异常码 | 异常助记符 | 异常处理函数 | 主要逻辑函数 | 大致逻辑
:-: | :-: | :-: | :-: | :--
0 | Int | `handle_int`(lib/genex.S) | - | 根据中断的类型来处理: 若为时钟中断, 则转入 `timer_irq`
1 | Mod | `handle_mod`(lib/genex.S) | `page_fault_handler`(lib/traps.c) | 此函数用于COW机制, 将在后续文章中介绍, 此处不作解释
2/3 | TLBL/TLBS | `handle_tlb`(lib/genex.S) | `do_refill` | 在 `mCONTEXT` 指示的页表中寻找虚地址匹配的实地址, 通过TLB相关指令填入TLB中.
8 | Syscall | `handle_sys`(lib/syscall.S) | - | 根据系统调用号来处理系统调用: 调用相应的系统函数

### 保存现场

MIPS-R3000的现场, 是指其寄存器的值 - PC, GRF, CP0, HI, LO的值. 在MOS中, 采用 `struct Trapframe` 结构来存储现场, 也就是 `struct Env` 中的 `env_tf` 段.

当前进程被中断异常打断时, 需要先使用汇编宏 `SAVE_ALL` (位于include/stackframe.h)**保存现场**, 然后再**进行中断异常处理**.
> 由于处理中断异常的过程仍然需要使用CPU中的各种寄存器, 因此需要先将现场暂存, 处理完毕后再恢复现场.

#### 获知异常处理栈

在保存现场之前, 需要解决一个重大问题: 保存到什么地方(异常处理栈)? MOS代码中include/stackframe.h中的汇编宏 `get_sp` 给出了答案.

```powershell
.macro get_sp
    mfc0    k1, CP0_CAUSE
    andi    k1, 0x107c
    xori    k1, 0x1000
    bnez    k1, 1f
    nop
    li      sp, 0x82000000
    j       2f
    nop
1:
    bltz    sp, 2f
    nop
    lw      sp, KERNEL_SP
    nop
2:
    nop
.endm
```

> 这里使用 `1f/2f` 简易标签来指示跳转地址: `1f` 中 `f` 表示 forward, 意为后面的 `1`; `2f` 同理.

此处给出该宏的简要解释:
1. 首先获取Cause寄存器的值, 存入 `k1`.
2. **若发生了4号中断(4号中断线接时钟, 即时钟中断), 则设置 `sp` 寄存器为 `0x82000000` (即 `TIMESTACK`); 否则设置 `sp` 寄存器为 `KERNEL_SP`(此为一个全局变量, 用于保存此时内核栈的栈顶, 初始化于lib/kclock_asm.S的 `set_timer` 汇编函数中).**
   > 按照 `KERNEL_SP` 的语义, 其初始化时应当指示内核栈顶, 因此在 `mips_init` 中最后一个函数处初始化.

接下来就可以将现场信息保存到 `sp` 指示的异常处理栈中了.
> **`TIMESTACK` 仅在时钟中断时被使用, 这其实是与进程切换达成了"协议"**: 因为时钟中断的处理函数就是 `sched_yield` , 最终在 `env_run` 时将 `TIMESTACK` 中保存的现场存入当前进程的 `env_tf` 中.

#### 保存现场到异常处理栈

保存现场的主要逻辑处于汇编宏 `SAVE_ALL` 中, 主要逻辑如下:

```powershell
.macro SAVE_ALL
    move    k0, sp
    get_sp
    move    k1, sp
    subu    sp, k1, TF_SIZE
    sw      k0, TF_REG29(sp)
    sw      $2, TF_REG2(sp)
    mfc0    v0, CP0_STATUS
    sw      v0, TF_STATUS(sp)
    mfc0    v0, CP0_CAUSE
    sw      v0, TF_CAUSE(sp)
    mfc0    v0, CP0_EPC
    sw      v0, TF_EPC(sp)
    mfc0    v0, CP0_BADVADDR
    sw      v0, TF_BADVADDR(sp)
    mfhi    v0
    sw      v0, TF_HI(sp)
    mflo    v0
    sw      v0, TF_LO(sp)
    sw      $0, TF_REG0(sp)
    sw      $1, TF_REG1(sp)
    //sw      $2, TF_REG2(sp)
    sw      $3, TF_REG3(sp)
    sw      $4, TF_REG4(sp)
    sw      $5, TF_REG5(sp)
    sw      $6, TF_REG6(sp)
    //...
    sw      $26, TF_REG26(sp)
    sw      $27, TF_REG27(sp)
    sw      $28, TF_REG28(sp)
    //sw      $29, TF_REG29(sp)
    sw      $30, TF_REG30(sp)
    sw      $31, TF_REG31(sp)
.endm
```

* 首先将用户栈指针存入 `k0` , 然后使用 `get_sp` 获取处理栈指针, 存入 `sp`.
* 将处理栈顶下移一个 `struct Trapframe` 的大小(`TF_SIZE` 为 `sizeof(struct Trapframe)`).
* 将用户栈指针 `k0` 存入 `Trapframe` 中的 `TF_REG29` 处.
* 借助 `v0` 寄存器将CP0寄存器以及HI, LO寄存器的值存入 `Trapframe` 中的对应位置.
  > * 注: `v0` 寄存器即 `$2`.
  > * 由于借助 `v0` 这个用户态下会用到的寄存器, 因此必须先将用户态下 `v0` 的值存入 `Trapframe` 中.
* 将GRF寄存器存入 `Trapframe` 的对应位置.
  > 由于 `$2` (`v0` 寄存器) , `$29` (`sp` 寄存器) 都已经保存了, 所以此处跳过它们.

### 恢复现场与回滚

恢复现场的主要逻辑由汇编宏 `RESTORE_SOME` (include/stackframe.h)支持. 此宏将现场中除了 `sp` 寄存器之外的所有寄存器都恢复到CPU中:

```powershell
.macro RESTORE_SOME
    .set mips1
    lw      v0, TF_STATUS(sp)
    mtc0    v0, CP0_STATUS
    lw      v1, TF_LO(sp)
    mtlo    v1
    lw      v0, TF_HI(sp)
    mtlo    v0
    lw      v1, TF_EPC(sp)
    mtc0    v1, CP0_EPC
    lw      $31, TF_REG31(sp)
    lw      $30, TF_REG30(sp)
    //lw      $29, TF_REG29(sp) <~~ $29 <=> sp
    lw      $28, TF_REG28(sp)
    //lw      $27, TF_REG27(sp) <~~ $27 <=> k1
    //lw      $26, TF_REG26(sp) <~~ $26 <=> k0
    lw      $25, TF_REG25(sp)
    lw      $24, TF_REG24(sp)
    // ...
    lw      $1, TF_REF1(sp)
.endm
```

> 此处并未恢复 `k0/k1` 这两个内核态寄存器, 这是**由于在 MIPS 规范下, 用户程序不会使用这两个寄存器. 而用户程序的编写者也应该遵循这个规范.**
> * 若需要直接编写汇编代码, 应当时刻遵循此规范.
> * 若通过编译器得到汇编代码, 则应当选择符合规范的编译器, 如: mips-4KC.

> 官方代码中 SR 寄存器的恢复过程繁杂, **但简化成上述代码后系统仍能正常工作**, 猜测官方代码中繁杂的 SR 寄存器恢复过程为冗余代码.

通过 `RESTORE_SOME` , 可以使得用户现场中, 除了 `sp` 栈指针寄存器外的信息都被恢复, 而 `sp` 寄存器将在回滚现场前的最后关头才恢复. **MOS中定义了汇编函数 `ret_from_exception` (lib/genex.S)用于恢复现场并回滚用户态**.

```powershell
FEXPORT(ret_from_exception)
    .set noat
    .set noreorder
    RESTORE_SOME
    lw      k0, TF_EPC(sp)
    lw      sp, TF_REG29(sp)
    jr      k0
    rfe
```

此汇编函数首先使用 `RESTORE_SOME` 将除了 `sp` 外的寄存器得到恢复. 随后将栈指针恢复到用户进程栈, 再**通过R3000标准回滚指令序列 `jr k0 + rfe` 恢复用户态**.
> R3000 中没有提供 `eret` 指令, 而仅仅提供了 `rfe` 指令. 详情参看 R3000 文档.

### 中断异常处理例程的搭建

有了前面的铺垫, 就可完整地理解中断处理全过程了.

此处主要讲述 `handle_int/mod/tlb` 系列函数的编写思路.

> 在官方代码中, 下述处理均使用了汇编宏 CLI. 但经测试, CLI 应属于冗余代码, 无任何用处, 故此处略去 CLI 相关的调用与解释.

#### `handle_mod/tlb` - 通用异常处理

**通用的异常处理框架在lib/genex.S中以汇编宏形式给出**:

```powershell
.macro BUILD_HANDLER exception handler clear
    .align 5
NESTED(handle_\exception, TF_SIZE, sp)
    SAVE_ALL
    .set at
    move    a0, sp
    jal     \handler
    nop
    j       ret_from_exception
    nop
END(handle\exception)
.endm

// ...

BUILD_HANDLER mod page_fault_handler cli
BUILD_HANDLER tlb do_refill cli
```

MOS 作者在编写异常处理框架时提取了上述共通部分, 形成了宏:
1. `SAVE_ALL`: 保存现场.
2. `move a0, sp`, `jal \handler`: 以 `sp` (即异常处理栈)为参数, 调用处理函数.
3. `j ret_from_exception`: 从处理函数返回后, 调用 `ret_from_exception` 汇编函数返回用户态.

$$
\begin{CD}
    \texttt{SAVE\_ALL}@=\texttt{get exception-stack \& save all registers}\\
    @VVV\\
    \texttt{jal \textbackslash handler}\\
    @VVV\\
    \texttt{j ret\_from\_exception}@=\texttt{restore registers \& return to user-space}
\end{CD}
$$

#### `handle_int` - 中断的处理

该处理函数的主要逻辑如下:

```powershell
    .set noreorder
    .align 5
NESTED(handle_int, TF_SIZE, sp)
    SAVE_ALL
    .set at
    mfc0    t0, CP0_CAUSE
    mfc0    t2, CP0_STATUS
    and     t0, t2
    andi    t1, t0, STATUS_IP4
    bnez    t1, timer_irq
    nop
END(handle_int)
timer_irq:
    j       sched_yield
    nop
```

主要效果为: 查Cause寄存器, 通过掩码操作, 获知当前的中断线. 若为4号中断(时钟中断), 则直接调用 `sched_yield`.
> * 由于当前 MOS 仅支持 4 号中断, 因此此处仅对 4 号中断进行了识别.
> * 若需要增加中断类型(如键盘中断), 则需要在此处进行额外的掩码判定.

> 前文中并未说明时钟是如何被开启的, 但这一点在代码中是明确的: 在 `mips_init` 中通过 `kclock_init` 来启动定时器.

**值得注意的是, 此处使用不具备链接效果的 `j` 指令调用 `sched_yield`, 这意味着时钟中断的恢复现场并不在此. 事实上, 时钟中断的现场恢复过程是在汇编函数 `env_pop_tf` 中, 而不是 `ret_from_exception`**.

$$
\begin{CD}
    \texttt{timer\_irq}\\
    @VVV\\
    \texttt{sched\_yield}\\
    @VVV\\
    \texttt{env\_run}\\
    @VVV\\
    \texttt{env\_pop\_tf(return from exception here)}
\end{CD}
$$

## MOS大厦的基石

至此, 可以讲MOS系统中最基础的软件功能已经实现: 启动初始化, 内存管理, 进程运行与管理, 中断异常处理. 这里再从大方向上回顾一下MOS系统的基础软件.

### MOS启动

MOS启动后首先进入 `_start` (boot/start.S) 中, 进行基本的CPU初始化工作, 随即调用 `main` (init/main.c). `main` 中又直接调用 `mips_init` (init/init.c).

在 `mips_init` 中, 依次调用了下述函数:
1. `mips_detect_memory`: 初始化 `maxpa`, `npage`, `basemem`, `extmem` 并给出系统内存的基本信息.
2. `mips_vm_init`: **为 `pages` 数组, `envs` 数组分配内存, 并将它们映射到用户空间的 `UPAGES` , `UENVS` 区域, 制作"模板页目录" `boot_pgdir`**.
3. `page_init`: **将 `pages` 数组中的可用的单元(表征物理页框)挂入 `page_free_list` 中**.
4. `env_init`: **将 `envs` 数组中的可用单元(表征进程)挂入 `env_free_list` 中, 且初始化 `env_sched_list`**.
5. `ENV_CREATE`: 为MOS系统创建启动后的第一个进程.
   > **注意此时时钟尚未开启, 因此该进程仍处于休眠状态**.
6. `trap_init`: 制作中断异常分发表.
7. `kclock_init`: 启动时钟.

随后内核的初始化结束, 进入死循环中.

### 第一次把CPU交给进程

**内核初始化结束后, 就进入了死循环, 但此时的死循环并非真正的死循环**. 当第一个时钟中断发出后, CPU立即陷入内核态, PC程序计数器从虚地址 `0x80000080` (kseg0段, 直接对应到物理地址 `0x00000080`)开始读取机器码. 而**在 `0x80000080` 处"等待"CPU的, 就是异常分发例程(`except_vec3`, 位于boot/start.S)**.

异常分发例程根据此时Cause寄存器中的ExcCode(时钟中断的ExcCode为 `0`), 跳转至 `handle_int` 处. `handle_int` 识别到时钟中断, 便调用 `sched_yield` . 此函数找到可运行的进程, 并将它的信息放入CPU中, 通过 `env_pop_tf` 将CPU交给此进程.

### 多个进程的切换

如果 `env_sched_list` 中存在多个进程, 则会通过时间片调度 `sched_yield` 的方法, 分时占用CPU.

> 某进程执行的过程中, 遇到时钟中断, 将按照[上面](#第一次把cpu交给进程)所说的流程进行进程调度.

### TLB缺失

**进程执行过程中, 难免遇到发出的虚地址无法在TLB中找到的困境. 此时会触发TLB缺失异常(TLBL/TLBS).**

R3000识别到此异常后, 将PC转至 `0x80000000` (不是上述的 `0x80000080`). 但由于MOS在此处并没有代码, 故CPU空转32周期后又到达位于 `0x80000080` 的 `except_vec3`. 根据其异常码, 跳转至 `handle_tlb` 处. **`handle_tlb` 本质上调用的是 `do_refill` , 便开始TLB重填例程**: 根据 `mCONTEXT` 所指示的页目录所在地址, 将引发TLB缺失的虚地址对应的实地址存入EntryLo, 通过 `tlbwr` 将此虚实地址映射关系存入TLB中.

> 当然, TLB 重填的过程相当复杂, 也有可能引发 `page_out` , 触发系统故障或被动插页等.