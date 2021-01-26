---	
layout:     post	
title:      『C Programming』 Memory Space And Allocation	
subtitle:   『C 程序设计』 内存空间与分配    
date:       2021-01-18	   
author:     Coekjan 
header-img: img/post-bg-CP.jpg	
catalog:    true    
tags:	
    - C Programming  
---

## 内存空间

![]({{ '/img/CP-Mem-Space.svg' | prepend: site.baseurl}})

## 静态内存分配 - 编译时分配

* 全局变量，全局数组，静态变量，常量均存于已初始化数据段或未初始化数据段
* 局部变量，局部数组均存于栈上；此外，函数调用的信息也存于栈上

**示例如下**:

```c
#include <stdio.h>
int global_initialized_variable = 1; // 全局变量，存于已初始化数据段
int global_uninitialized_variable; // 全局变量，存于未初始化数据段，自动初始化为 0
char * global_string_constant = "Coekjan";
    /*
     * global_string_constant 是全局变量，存于已初始化数据段
     * "Coekjan" 是只读的静态字符串，存于已初始化数据段的常量区
     */
char global_char_array[] = "Hey!";
    /*
     * 该定义等价于
     * char global_char_array[] = {'H', 'e', 'y', '!', '\0'};
     * 实质上是定义了一个全局数组，存于已初始化的数据段，是可读可写的！!
     * 请注意与上一个声明方法进行区分。
     */
int global_array[100]; // 全局数组，存于未初始化的数据段

void func(char func_argument) {
    /*
     * func 是函数，其入口地址处于代码段
     * func_argument 是局部变量，存于栈
     */
    static int static_variable; // 静态变量，存于未初始化数据段
    int local_uninitialized_variable; // 局部变量，存于栈
    int local_uninitialized_array[100]; // 局部数组，存于栈
    /*
    局部变量与局部数组若不进行初始化，其值是不可预测的。
    */
    putchar(func_argument);
}

int main() { // main 是函数，其入口地址处于代码段 
    void (*func_ptr)(char) = func;
    /*
     * 局部变量，存于栈
     * 把 func 理解为函数的入口地址，func_ptr 就是指向了这个入口的函数指针
     * 因此，func_ptr 本身存于栈上，而存储的值是代码段中的某处地址
     */
    func_ptr('x'); // 相当于 func('x')
    return 0;
}
```

## 动态内存分配 - 运行时分配

动态内存分配是在程序运行时在堆上进行的内存分配。

C 程序中常常使用`malloc`与`calloc`进行动态内存分配。动态内存分配有以下需要注意的地方：

**动态分配的内存必须使用`free`进行释放**. 分析如下程序：

```c
#include <stdlib.h>
int main() { // 危险行为，请勿轻易尝试
    char *ptr;
    for(;;) {
        ptr = (char *)malloc(sizeof(char));
        *ptr = 0;
    }
    return 0;
}
```

该程序死循环中不断使用`malloc`申请内存，并写内存，**但没有使用`free`函数释放这片内存**, 因此运行时，内存占用越来越高，最终由于内存不足，使得`malloc`返回`NULL`, 此时`*ptr = 0;`将向`0x0`地址写，引发异常。

**一般而言，堆上分配内存（动态分配）的效率比栈上分配内存（静态分配）的效率低**. 因此如果能够确定对象的最大空间需求，静态内存分配是较好的选择。下面的程序使用静态分配的数组模拟了链表的插入函数（结点数不大于 100):

```c
#define <stdio.h>
#define <stdlib.h>

struct node {
    int data;
    struct node *next;
};
struct node list[100];
struct node *head = NULL;
int list_len = 0;

struct node *insert(int data, struct node *pre) {
    if (list_len >= 100) return NULL; // 内存不足 
    struct node *new_node = &list[list_len++]; // 在数组中"申请"内存
    /*
     * 动态内存分配的写法是：
     * struct node *new_node = (struct node *)malloc(sizeof(struct node));
     */
    new_node->data = data;
    if (pre == NULL) { // 如果 pre 为 NULL, 则新结点添加至链表头 
        new_node->next = head;
        head = new_node;
    } else { // 否则，新结点添加至 pre 后 
        new_node->next = pre->next;
        pre->next = new_node;
    }
    return new_node;
}
```

## 查看一个对象的地址

由于 C 语言提供了单目的`&`运算符用于取地址，而地址本身是一个数据，因此可以使用`printf`函数输出某对象的地址（或首地址）.

```c
#include <stdio.h>
int main() {
    int a = 2;
    int *p = &a;
    printf("Value of a = %x\n", a); // 2
    printf("Addr of a = %x\n", &a); // 62fe1c
    printf("Value of p = %x\n", p); // 62fe1c
    printf("Addr of p = %x\n", &p); // 62fe10
    return 0;
}
/* 注释为本地测试结果，不同机器的输出可能会有所不同。一些现象的解释如下：
 * 1. a 与 p 均为局部变量，存于栈上，且均占 4 字节，因此 p 的地址比 a 的地址低 4
      （栈由高地址往低地址方向延伸）.
 * 2. p 本身存储了 a 的地址，因此 p 的值与 a 的地址相同。
 */
```

### 使用宏函数方便查看对象的地址和值

你可以像如下的示例一样编写宏函数，方便查看对象的地址和值。下面的程序运行结果与上面的程序运行结果无区别。

```c
#include <stdio.h>
#define printAddr(V) printf("Addr of " #V " = %x\n", &V)
#define printValue(V) printf("Value of" #V " = %x\n", V)

// 上述宏定义中 #V 是字符串化

int main() {
    int a = 2;
    int *p = &a;
    printValue(a);
    printAddr(a);
    printValue(p);
    printAddr(p);
    return 0;
}
```

> 使用 debug 工具也可以查看对象的地址和值，此处使用宏函数纯属是拓宽思路。