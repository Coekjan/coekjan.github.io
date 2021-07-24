---	
layout:     post	
title:      『MOS』 Boot And Memory Management	
subtitle:   『MOS』 启动与内存管理    
date:       2021-07-23	   
author:     Coekjan 
header-img: img/post-bg-MOS.jpg	
catalog:    true	
katex:    true    
tags:	
    - MOS  
---

## 启动与初始化

MOS启动时, PC首先转至启动代码 `.text` 段的首地址. 随后由下述汇编代码, 对硬件进行初始化, **其中最重要的作用是**:
* **禁用全局中断**: 初始化内核时, 不应响应中断.
* **初始化栈指针**: 内核态下的函数调用, 需要利用内核下的栈, 此处将栈底设定为 `0x80400000`.
* *初始化页目录基地址*: 内核处于 kseg0 段, 本不需要通过页表和 TLB 进行虚实地址转换. 此处设定内核下的页目录, 并在初始化时填写该内核页表, 这个页表将在后续进程部分用到.

```powershell
    .set    at
LEAF(_start)
    .set    mips2
    .set    reorder
    /* Disable interrupts */
    mtc0    zero, CP0_STATUS
    /* set up stack */
    li      sp, 0x80400000
    /* set up page-table */
    li      t0,0x80400000
    sw      t0,mCONTEXT
    /* jump to main */
    jal     main
    nop
loop:
    j       loop
    nop
END(_start)
```

> 注意, 官方代码中除了上述代码外, 还有关于 Watch, Config 寄存器的代码. *但笔者翻阅 R3000 手册后, 并未找到确切依据证明其有效性. 实践中, 笔者将这两个寄存器的初始化代码删去后仍能运行 Shell, 故此处**或许**为官方的冗余代码.*

经由 `jal main` 跳转至定义于init/main.c中的 `main` 函数. `main` 函数随即调用定义于init/init.c中的 `mips_init` 函数.

```c
void mips_init() {
    mips_detect_memory();
    mips_vm_init();
    page_init();
    // ...
    trap_init();
    // ...
}
```

> `trap_init` 在上一篇文章中提及, 用于异常分发.

* `mips_detect_memory`: 初始化内存信息.
* `mips_vm_init`: 初始化内核中重要变量
  * `freemem`: 第一次调用 `alloc` 函数时, 将被初始化为虚地址 `end` (`end` 定义于tools/scse0_3.lds中, 其本质上是 `0x80400000`). **`PADDR(freemem)` 指示了当前内核已占用的物理内存**.
    > `alloc` 函数将按页 (4KB) 分配物理内存.
  * **`pgdir`/`boot_pgdir`**: 内核页目录基地址.
  * `mCONTEXT`: 记录当前进程的页目录基地址, 由于此时处于内核初始化阶段, 因此将其初始化为内核页目录基地址.
  * **`pages` 数组**: 用于进行物理页框分配管理, 这里使用的是链表法进行物理页框分配.
  * `envs` 数组: 用于进行进程控制块分配管理, 后续的文章中介绍其作用.
* `page_init`: **将 `pages` 数组中的单元挂入链表中**, 后续 `page_alloc`, `page_free` 等操作都要基于链表实现.
* 除了这几个与内存管理相关的函数外, `mips_init` 中还有其他的重要内容, 这些内容都将在后续的文章中逐一揭晓.

再次强调, 根据tools/scse0_3.lds的描述, **启动与初始化的过程中的函数调用栈, 全局变量, 代码段均存在与kseg0段**. 在未完成内存管理时, 不可访问Mapped的内存空间(kuseg, kseg2). **MOS中通过kseg0来完成内存管理, 之后便可以访问Mapped的内存空间了**. 利用kseg0中的虚实地址对应特性, 可以通过下述两个宏进行虚实地址的线性变换:

```c
#define PADDR(kva)                                                      \
    ({                                                                  \
        u_long a = (u_long)(kva);                                       \
        if (a < ULIM) /* ULIM = 0x80000000 */                           \
            panic("PADDR called with invalid kva %08lx", a);            \
        a - ULIM;                                                       \
    })
#define KADDR(pa)                                                       \
    ({                                                                  \
        u_long ppn = PPN(pa); /* PPN(a) <==> (((u_long)(a)) >> 12) */   \
        if (ppn >= npage) /* npage = 0x4000 */                          \
            panic("KADDR called with invalid pa %08lx", (u_long)(pa));  \
        (pa) + ULIM;                                                    \
    })
```

> 这里使用了 GCC 中的 C 语法扩展 `({ ... })` .

## 物理内存管理

在MOS实验中, 采用4KB的物理页框和虚拟页面大小. 实验中采用的内存大小为64MB, 因此总共有16K(即16×2<sup>10</sup>)个物理页框. 欲对系统资源进行管理, 通常可采用**链表法或位图法**等实现. 值得注意的是, 无论采用什么方法进行管理系统资源, **只要对外的接口实现正确, 就可使得系统正常工作**. MOS实验中, 物理内存管理的最重要对外接口是:
* `page_init`: 初始化物理内存管理
* `page_alloc`: 申请一个物理页框
* `page_free`: 释放一个物理页框

**MOS实验中, 采用链表法进行实现物理内存管理**. 空闲链表定义为:

```c
static struct Page_list page_free_list;
```

### `Page` 结构体与物理页框的关系

`Page` 结构体本身并不占用4KB的大小, 仅仅只是用一个 `Page` 结构体来代表一个物理页框. 可以通过下面这些定义于include/pmap.h中的函数将某一 `Page` 与相应的物理页框首地址进行相互转换:

```c
extern struct Page *pages; // initialized in `mips_vm_init`

// convert `struct Page *` to physical page number
static inline u_long page2ppn(struct Page *page_ptr) {
    return page_ptr - pages;
}
// convert `struct Page *` to physical address
static inline u_long page2pa(struct Page *page_ptr) {
    return page2ppn(page_ptr) << PGSHIFT; // PGSHIFT <==> 12
}
// convert physical address to `struct Page *`
static inline struct Page *pa2page(u_long physical_addr) {
    if (PPN(physical_addr) >= npage)
        panic("pa2page called with invalid pa: %x", physical_addr);
    return &pages[PPN(physical_addr)];
}
// convert `struct Page *` to kernel virtual address
static inline u_long page2kva(struct Page *page_ptr) {
    return KADDR(page2pa(page_ptr));
}
```

**定理**: 某一 `Page` 结构体被"占用", **当且仅当**对应的物理页框被占用.

> 称一个 `Page` 结构体被 "占用", 是指结构体不存在于空闲链表中.

### 初始化物理内存管理 - `page_init`

主要分为 3 个步骤:
1. 初始化 `page_free_list`: 使用 `LIST_INIT(&page_free_list)` 即可.
2. 将已被占用的物理页面, 对应的 `Page` 结构体中的 `ref` 字段设置为 `1`: 即将这些物理页面标识为**被映射了 `1` 次**. 此处的被映射, 是指它们都**被对应的虚拟地址直接线性映射**(内核虚地址最高位抹去就访问到对应的物理地址).
   > `ref` 字段的含义为, 计数该物理页框被多少个虚拟页面映射.
3. 将未被占用的物理页面插入到空闲链表中.

> 这里注意, **在前面的各种初始化工作中, 已经消耗了一些物理内存**(如函数调用栈, 内核页表等). 那么**如何才能知道现在有多少物理内存被占用了呢? 答案就是 `freemem` .**

### 链表法管理物理页框 - `page_alloc` & `page_free`

#### `page_alloc`

`page_alloc` 用于将一个物理页面分配出去, 分配的结果就是参数中的二重指针被指向为对应的 `Page` 结构体地址.
1. 首先检查空闲链表中是否为空(`LIST_EMPTY(&page_free_list)`), 若为空则**返回负值表示异常**.
2. 若非空, 则从链表头部结构体指针(`LIST_FIRST(&page_free_list)`)并在链表中移除之, 将该页面初始化为全 `0x00000000` .
3. 最后将二重指针指向该结构体指针, 返回 `0` .

> MOS 系统中, 往往使用负返回值表示异常, 零返回值表示正常. 异常码均定义于 include/mmu.h 中.

大致的框架为:

```c
int page_alloc(struct Page **dbl_ptr) {
    struct Page *tmp;
    if (/* I. `page_free_list` is empty */) return -E_NO_MEM;
    // negative return value indicates exception.

    tmp = /* II. the first item in `page_free_list` */;

    /* III. remove this page from the list */;

    bzero(/* IV. kernel virtual address of this page */, BY2PG);

    *dbl_ptr = tmp;
    return 0;
    // zero indicates success.
}
```

值得注意的是, **此处 `bzero` 的第一个参数为内核虚地址**, 这是因为我们的**程序中涉及访存的地址归结起来都是CPU发出的地址. 只要是CPU发出的地址, 就必须是虚地址.**

> 建议参考阅读 `bzero` 的源码.

在调用 `page_alloc` 后, 往往需要将这个 `Page` 结构体中的 `ref` 增一.

#### `page_free`

`page_free` 接受一个 `Page` 结构指针为参数, 若其中的 `ref` 字段为 `0` , 则将其重新插入到 `page_free_list` 中.

大致的框架为:

```c
void page_free(struct Page *page_ptr) {
    if (page_ptr->pp_ref == 0) {
        /* I. insert this item into `page_free_list` */
        return;
    } else if (page_ptr->pp_ref > 0) return; // in use

    panic("pp_ref is less then 0");
}
```

在调用 `page_free` 前, 往往需要将这个 `Page` 结构体中的 `ref` 减一.

## 虚拟内存管理

MOS实验中, 采用**二级页表**来实现虚拟内存. 在 `mips_vm_init` 中, 调用的 `boot_map_segment` 就是在填写页表, 将某一虚空间映射到对应的实空间. 这里需要重点了解的是:
1. **填写二级页表的关键步骤**.
2. 在没有硬件MMU的R3000上, 使用**软件填写TLB的大致过程**.

### 二级页表的填写

欲将某一虚拟页面映射到具体的物理页框, **最终是需要通过TLB来完成的**. 在填写TLB的过程中, 需要**访问页表, 从页表中查找该虚拟页面对应的物理页框号和对应的权限位**. 这里先介绍二级页表的填写过程.

> 注意, MOS 中的页表存储于内存中, 而不是存储于硬件 MMU 中.
> > R3000 没有硬件 MMU!

![]({{ '/img/MOS-Fill-In-Page-Table.svg' | prepend: site.baseurl}})

#### `page_init` 前的映射建立方法

> 涉及函数 `boot_pgdir_walk` , `boot_map_segment` .

在 `page_init` 被调用之前, 物理内存管理尚未完成, 因此无法调用 `page_alloc`, `page_free` 函数进行物理内存分配与释放. 但这之前使用的内存空间也需要建立虚实地址映射(`boot_map_segment`).

因此, 在上面的图示中, 使用 `boot_pgdir_walk` 来完成Step 1; 在 `boot_map_segment` 中完成Step 2. 其中 `walk` 的过程中需要使用 `alloc` 来创建页表.

大致框架为:

```c
static Pte *boot_pgdir_walk(Pde *pgdir, u_long va, int create) {
    Pde *pgdir_entry = pgdir + PDX(va); // I

    // check whether the page table exists
    if ((*pgdir_entry & PTE_V) == 0) {
        if (create) {
            /**
             * use `alloc` to allocate a page for the page table
             * set permission: `PTE_V | PTE_R`
             * hint: `PTE_V` <==> valid ; `PTE_R` <==> writable
             */
        } else return 0; // exception
    }

    // return the address of entry of page table
    return ((Pte *)(/* II. Kernel Virtual Address of PTBase */)) + PTX(va);
}
void boot_map_segment(Pde *pgdir,
                      u_long va, u_long size,
                      u_long pa, int perm) {
    int i;
    Pte *pgtable_entry;
    for (i = 0, size = ROUND(size, BY2PG); i < size; i += BY2PG) {
        /* Step 1. use `boot_pgdir_walk` to "walk" the page directory */
        pgtable_entry = boot_pgdir_walk(
            pgdir,
            va + i,
            1 /* create if entry of page directory not exists yet */
        );
        /* Step 2. fill in the page table */
        *pgtable_entry = (/* III. Physical Frame Address of `pa + i` */)
                         | perm | PTE_V;
    }
}
```

#### `page_init` 后的映射建立方法

> 涉及函数 `pgdir_walk` , `page_insert` , `page_remove` 等. 其中由 `page_insert` 调用 `pgdir_walk` .

值得注意, 在 `page_init` 被调用后, 这些函数(`page_insert`, `page_remove`)都不会被立即调用. **这些函数只是在进程创建, 进程执行中和进程死亡的进程生命周期中被调用**. 也就是说, 这些函数都是用来**为进程建立虚实地址映射的**.
* `page_insert`: 为给定的页目录, 在页表中建立给定虚地址与给定物理页面之间的映射.
* `page_remove`: 为给定的页目录, 在页表中取消给定虚地址的映射.

在上面的图示中, 使用 `pgdir_walk` 完成Step 1; 使用 `page_insert` 完成Step 2. 大致框架为:

```c
int pgdir_walk(Pde *pgdir, u_long va, int create, Pte **ppte) {
    Pde *pgdir_entry = pgdir + PDX(va); // I
    struct Page *page;
    int ret;
    
    // check whether the page table exists
    if ((*pgdir_entry & PTE_V) == 0) {
        if (create) {
            if ((ret = page_alloc(&page)) < 0) return ret;
            *pgdir_entry = (/* Physical Address of `page` */)
                           | PTE_V | PTE_R;
        } else {
            *ppte = 0;
            return 0;
        }
    }
    *ppte = ((Pte *)(/* II. Kernel Virtual Address of PTBase */)) + PTX(va);
    return 0;
}
int page_insert(Pde *pgdir, struct Page *pp, u_long va, u_int perm) {
    Pte *pgtable_entry;
    int ret;

    perm = perm | PTE_V;

    // Step 0. check whether `va` is already mapping to `pa`
    pgdir_walk(pgdir, va, 0 /* for check */, &pgtable_entry);
    if (pgtable_entry != 0 && (*pgtable_entry & PTE_V) != 0) {
        // check whether `va` is mapping to another physical frame
        if (pa2page(*pgtable_entry) != pp) {
            page_remove(pgdir, va); // unmap it!
        } else {
            tlb_invalidate(pgdir, va);              // <~~
            *pgtable_entry = page2pa(pp) | perm;    // update the permission
            return 0;
        }
    }
    tlb_invalidate(pgdir, va);                      // <~~
    /* Step 1. use `pgdir_walk` to "walk" the page directory */
    if ((ret = pgdir_walk(pgdir, va, 1, &pgtable_entry)) < 0)
        return ret; // exception
    /* Step 2. fill in the page table */
    *pgtable_entry = (/* III. Physical Frame Address of `pp` */) | perm;
    pp->pp_ref++;
    return 0;
}
```

这里使用到 `page_remove` , `tlb_invalidate` 这两个函数. 其中前者是用来消除映射的, 逻辑较为简单且不是本文的重点, 因此略去; 后者用于**将TLB中对于 `va` 的原有映射条目抹去**.

回顾一下TLB的作用, TLB用于缓存虚实地址映射关系. **从硬件角度上看, TLB中的内容不会随着页表内容而变化**, 因此当页表内容发生变化时需要**使用软件重新填写TLB**. **MOS中的思路是: 页表内容改变时(`page_insert` 和 `page_remove` 时)清除TLB中的相关条目, 当进程访问该虚地址时, 引发TLB缺失异常, 由TLB缺失处理函数 `handle_tlb` 来填写TLB.**

`tlb_invalidate` 的原理是以虚地址与进程的ASID为参数调用汇编函数 `tlb_out` :

> 在 TLB 中查找表项时, 需要提供 VPN 和 ASID.

```c
void tlb_invalidate(Pde *pgdir /* unused? */, u_long va) {
    if (curenv) // a process is running
        tlb_out(PTE_ADDR(va) | GET_ENV_ASID(curenv->env_id));
    else
        tlb_out(PTE_ADDR(va));
}
```

`tlb_out` 其实就是查找并消除TLB表项:

```powershell
LEAF(tlb_out)
    mfc0        k1, CP0_ENTRYHI // save current EntryHI
    mtc0        a0, CP0_ENTRYHI
    // `tlbp` will probe the entry whose HI equals to EntryHI
    tlbp
    // `tlbp` costs clocks
    nop
    nop
    nop
    nop
    mfc0        k0, CP0_INDEX
    // now Index stores the index of the corresponding entry
    bltz        k0, NOFOUND
    // according to the behavior of `tlbp`:
    // not found <==> the highest bit of Index is `1` <==> negative
    nop
    mtc0        zero, CP0_ENTRYHI
    mtc0        zero, CP0_ENTRYLO0
    tlbwi
    // store zero into the entry whose index is Index
NOFOUND:
    mtc0        k1, ENTRYHI // restore EntryHi
    jr          ra
    nop
END(tlb_out)
```

> 这里使用了很多 TLB 相关的指令, 可以参照引言中的[介绍]({{ '/2021/07/15/Introduction/#tlb%E7%9B%B8%E5%85%B3%E6%8C%87%E4%BB%A4' | prepend: site.baseurl}})进行阅读.

### TLB的填写

上文提到, MOS中使用 `page_insert` 和 `page_remove` 使得页表条目发生变化时, 仅仅只是将对应的TLB表项清零. 当进程访问到该虚地址时, 将引发TLB缺失异常. 引言中提到, TLB缺失异常的异常码被绑定到 `handle_tlb` 这一处理函数(定义于lib/genex.S).

该汇编文件中, 使用了汇编宏 `BUILD_HANDLER` 来生成 `handle_tlb` 的代码, `handle_tlb` 最终落入 `do_refill` 函数完成TLB填写.

> `handle_tlb` 的前后过程比较复杂: 涉及到保存现场, 关闭中断, 设置栈指针, 恢复现场, 开启中断, 恢复栈指针等操作. **这些操作在此处不作解释**, 有兴趣的读者可以参阅源代码. 后续有关中断异常的文章中将重点介绍这些流程.
> 
> 此处只对其中最核心的 `do_refill` 例程进行简单解释.

`do_refill` 用于根据当前的页目录填写TLB项. 注意, 由于是TLB缺失时才会引发 `do_refill` , 因此此时**硬件已经做好了一些准备工作**:
* EntryHi已经被设置为引发TLB缺失的VPN与ASID.
* BadVaddr已经被设置为引发TLB缺失的虚地址.

大致流程为:
1. **确定此时的页目录**: `mCONTEXT` 中存储了当前的页目录虚地址;
2. 从BadVaddr中取出引发TLB缺失的虚地址, 并确定其对应的页目录索引(高10位);
3. 根据页目录索引, 从页目录中取出对应的表项: 此时取出的**页目录表项由二级页表的实地址与权限位组成**.
4. 判定权限位: 若权限位显示该表项无效(无 `PTE_V`), 则调用`page_out`, 随后结束;
5. 确定引发TLB缺失的虚地址对应的二级页表索引(中间10位);
6. **将二级页表的实地址转为内核虚地址**(高位补 `1`), 随后根据二级页表索引从页表中取出对应的表项: **此时取出的表项由实地址与权限位组成**.
7. 判定权限位: 若权限位显示该表项无效(无 `PTE_V`), 则调用 `page_out`, 随后结束;
8. 将实地址存入EntryLo, 并**调用 `tlbwr` 将此时的EntryHi与EntryLo写入到TLB中**.

这里调用了 **`page_out` 函数处理页表中找不到对应表项的异常**, 流程大致如下:
1. 若TLB缺失时, `mCONTEXT` 不处于内核空间, 则说明系统出现了故障: **页表应处于内核态中**.
2. 若引发TLB缺失的虚地址不合法(如访问了过低或过高的地址), 则说明系统故障.
3. 否则可以为此虚地址申请一个物理页面(`page_alloc`), 并将虚地址映射到该物理页面(`page_insert`). (被动扩充内存)

> 这里的两个函数都只进行了大致的解释, 详细的实现可以参阅源代码.
