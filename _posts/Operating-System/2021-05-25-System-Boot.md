---	
layout:     post	
title:      『Operating System』 System Boot	
subtitle:   『操作系统』 系统引导    
date:       2021-05-25	   
author:     Coekjan 
header-img: img/post-bg-OS.jpg	
catalog:    true    
tags:	
    - Operating System  
---

## Bootloader

> 启动前, 硬件的状态处于一个安全, 通用, 功能最弱的状态; 而启动, 就是要逐步设置硬件, 提升计算机系统的环境, 提高可用性与灵活性.

Bootloader是系统加点后, 运行的**第一段软件代码**, 是运行操作系统内核前的一段小软件:
* Boot: 初始化系统硬件, 使得系统硬件正常运行(至少是部分运行).
* Load: 将操作系统的内核载入内存, 并将执行权交给操作系统.

常见体系结构对应的Bootloader:

MIPS | X86
:-: | :-:
U-boot | LILO / GRUB

**需要注意的是, Bootloader的实现严重依赖于具体的硬件, 不可能有一个Bootloader能支持所有的CPU/开发板.**

## 启动与OS引导

### MIPS体系 (U-boot)

#### MIPS基本地址空间

![]({{ '/img/OS-MIPS-Base-Address-Space.svg' | prepend: site.baseurl}})

* **kuseg**: 用户态2GB地址, 将经过地址转换机构转换. 在地址转换机构设置好前, 此2GB空间是无法使用的.
* **kseg0**: 内核态第一段512MB地址, 不经过地址转换机构, 直接去掉最高位得到物理地址, 但通过Cache. 在Cache设置好前, 此512MB地址是无法使用的.
* **kseg1**: 内核态第二段512MB地址, 不经过地址转换机构, 直接去掉高三位得到物理地址, 不通过Cache. 这是系统重启时唯一能够工作的地址空间.
* **kseg2**: 最高的1GB地址, 经过地址转换机构, 通过Cache, 只允许在内核态使用.

#### MIPS系统启动

MIPS系统加电后, 从 `0xbfc00000` 这个入口地址开始执行, 这个地址位于加电后唯一能够工作的kseg1段, 对应的物理地址为 `0x1fc00000` .

![]({{ '/img/OS-MIPS-Boot.svg' | prepend: site.baseurl}})

#### MIPS下的Linux引导

首先, Bootloader将Linux内核镜像拷贝到RAM中, 将执行权交给Linux内核.

Linux内核启动的**第一个阶段从/arch/mips/kernel/head.s文件开始**. 此处正是内核入口函数 `kernel_entry` , 此函数是与体系结构相关的汇编函数, 它初始化内核堆栈, 为创建系统的第一个进程做准备, 接着用一段循环将内核映像中未初始化的部分清零, 最后转至/init/main.c中的 `start_kernel` 初始化硬件.

**第二阶段就是/init/main.c中的 `start_kernel`**. 包括设置CPU ID, 初始化内核数据结构, 初始化内存管理, 给多核处理器的多个核心分配物理地址空间, 初始化进程调度与时钟, 启用时钟中断, 控制台中断, 启用用户进程等工作.

### X86体系

#### 启动BIOS(Basic Input/Output System)

* **硬件自检POST(Power-On-Self-Test)**: BIOS首先检查计算机硬件是否满足运行的基本条件.
* **启动顺序BS(Boot Sequence)**: 硬件自检完毕后, BIOS将控制权转交给启动顺序中下一阶段的程序.

#### 主引导记录MBR(Master Boot Record)

计算机读取硬盘的第一个扇区(512B), 即为MBR: 0~446B为启动代码与数据, 447~510B为分区表, 511~512B为幻数 `0xAA` 与 `0x55` .

![]({{ '/img/OS-X86-MBR.svg' | prepend: site.baseurl}})

其中分区标识占1B, 若为 `0x80` 则表示该分区为激活分区, 控制权就交给这个分区. **四个分区中, 只有一个分区可以被激活.**

#### 硬盘启动

计算机读取激活分区的第一个扇区, 该扇区就是**卷引导记录VBR(Volume Boot Record)**, 指示操作系统的位置, 接着计算机就会加载这个操作系统.

#### X86下的Linux引导 - 以GRUB为例

首先, GRUB读MBR, 识别不同的文件系统格式. 紧接着加载Linux系统引导菜单(/boot/grub/menu.lst或grub.lst), 加载Linux内核镜像和RAM磁盘initrd(可选). 然后就可以开始运行Linux内核了.