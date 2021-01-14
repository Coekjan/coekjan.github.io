---	
layout:     post	
title:      『Computer Architecture』 Automatic Testing	
subtitle:   『计算机体系结构』 自动化测试    
date:       2021-01-15	   
author:     Coekjan 
header-img: img/post-bg-CA.jpg	
catalog:    true	
tags:	
    - Computer Architecture  
---

对于一个CPU工程而言, 保证其正确性是及极其重要的. 一般而言, 确保一个CPU工程的正确性的手段有两种:
* 形式化验证
* 随机样例测试

而本节将针对**随机样例**测试进行讲解. 我们采用如下方法进行测试:

![]({{ '/img/CPU_AutoTest-Process.svg' | prepend: site.baseurl}})

## 黄金模型MARS与命令行

MARS提供了内置的仿真CPU程序, 我们将以之为标准进行对拍测试.

MARS提供了命令行工具运行MIPS汇编程序, 具体可在**MARS->Help->MARS->Command**中查看. 下面列出一些本系列博文中最常用到的命令行:

### 汇编转机器码

```cmd
java -jar <mars> <assembly-file> [db]
    nc mc CompactDataAtZero a dump .text HexText code.txt

java -jar <mars> <assembly-file> [db]
    nc mc CompactDataAtZero a dump 0x00004180-0x00005180 HexText code_handler.txt
```

#### 有关参数说明

参数 | 属性 | 说明 | 示例
:---:|:---:|:---:|:---:
`<mars>` | 必选 | MARS的JAR包名称 | `Mars.jar`
`<assembly-file>` | 必选 | 汇编程序文件 | `TestPoint1.asm`
`[db]` | 可选 | 启用延迟槽 | `db`

### 汇编运行

为与CPU工程对拍, 我们需要对MARS进行一些修改:
* 指令写寄存器时, 输出信息`@PC : $Reg <= Data`, 其中PC是该指令的**PC的十六进制值**, Reg是**寄存器编号的十进制值**, Data是**写入数据的十六进制值**.
* 指令写内存时, 输出信息`@PC : *Addr <= Data`, 其中PC是该指令的**PC的十六进制值**, Addr是**内存地址的十六进制值**(地址字对齐), Data是**写入数据的十六进制值**(数据宽度为一字宽).

为进行这些修改, 可以参考[项目]()

修改完毕后, 命令行运行汇编程序即可得到黄金模型的输出

```cmd
java -jar <mars> <assembly-file> [db] nc mc CompactDataAtZero >std_ans.txt
```

有关参数说明参见[上文](#有关参数说明).

## 仿真工具ISim与命令行

ISE带有的ISim是仿真CPU工程的软件, 可以通过命令行运行CPU工程并得到输出.

> 约定: 顶层模块为`mips.v`, 针对顶层模块的测试文件为`mips_tb.v`

### PRJ文件与TCL文件

PRJ文件用于说明工程包含哪些模块, 使用绝对路径说明. 如若`D:\Project`文件夹下有工程:

```library
| mips.prj
| mips.v
| mips_tb.v
|-- datapath
  | datapath.v
  |-- module
    | module1.v
    | module2.v
|-- controller
  | decoder.v
  | main_controller.v
```

则需要在`mips.prj`中编写:

```prj
Verilog work "D:\Project\mips.v"
Verilog work "D:\Project\mips_tb.v"
Verilog work "D:\Project\datapath\datapath.v"
Verilog work "D:\Project\datapath\module\module1.v"
Verilog work "D:\Project\datapath\module\module2.v"
Verilog work "D:\Project\controller\decoder.v"
Verilog work "D:\Project\controller\main_controller.v"
```

TCL文件用于配置工程的运行参数, 如若工程运行200us后停止, 则需要在`mips.tcl`中编写:

```tcl
run 200us;
exit
```

注意: **PRJ文件与TCL文件都要放在工程目录下**.

> 可以编写一个简单的程序生成这两个文件

### 命令行

ISim命令行需要设置环境变量, 我们采用**Python3**来设置环境变量并进行命令行调用:

```python
import os
# ...
os.environ['XILINX'] = xilinx_path
os.system(xilinx_path + '\\bin\\nt64\\fuse -nodebug -prj mips.prj -o mips.exe mips_tb >log.txt')
os.system('mips.exe -nolog -tclbatch mips.tcl > test_ans.txt')
```

其中`xilinx_path`即为ISE的安装目录.

## 文件比对

通过MARS, 我们得到了标准输出; 通过ISim, 我们得到了测试输出. 只须编写一个简单的文件比对程序, 即可完成测试.
