---	
layout:     post	
title:      『C Programming』 CmpFunc In QSort	
subtitle:   『C程序设计』 快速排序中的比较函数    
date:       2021-03-20	   
author:     Coekjan 
header-img: img/post-bg-CP.jpg	
catalog:    true    
tags:	
    - C Programming  
---

## 标准库中的 `qsort`

在C语言的使用过程中, 往往会使用快速排序库函数 `qsort` 组织数据, 其声明如下:

```c
void qsort(
    void        *base,
    size_t      numOfEles,
    size_t      sizeOfEles,
    int         (*cmp)(const void*, const void*)
);
```

其中, 前三个参数分别为:
1. 数组首地址
2. 数组元素总个数
3. 数组单个元素字节数

这三个参数都仅与数组本身有关, 此处不再赘述.

### `cmp` 的编写方法

我们关注的是第四个参数 `cmp` , 该函数(指针)**定义了元素之间的"大小"关系**(二元序关系). 当传入两个参数 `n1` , `n2` 时:
* 若函数返回值**小于 `0`** , 则认为 `n1` **"小于"** `n2` ;
* 若函数返回值**大于 `0`** , 则认为 `n1` **"大于"** `n2` ;
* 若函数返回值**等于 `0`** , 则认为 `n1` **"等于"** `n2` .

`qsort` 将按照这个"大小"关系, "从小到大"排列数组中的元素.

下面举例, 以 `int` 型数据的降序关系编写该函数:

```c
int int_decrease(const void *n1, const void *n2) {
    int num1 = *(int *) n1;
    int num2 = *(int *) n2;
    if (num1 > num2) {
        return -1;
    } else if (num1 < num2) {
        return 1;
    } else {
        return 0;
    }
}
```

以第一个 `if` 分支为例: 当 `num1` **数值上大于** `num2` 时, 由于 `qsort` 将数据按照**所定义顺序的"从小到大"** 排列, 且为了使得 `int` 型数据递降排列, 应当认为 `num1` **"小于"** `num2` , 返回负数.

再以一个结构体数组为例, 定义其序关系:

```c
#define NAME_MAX 48
typedef struct person {
    char    name[NAME_MAX];
    int     age;
} person;

person list[10];
```

假设其中已存有 `10` 个 `person` 元素, 下面需按照如下的规则排序:
1. 若两个 `person` 的 `age` 不同, 则按照 `age` 的从大到小排序;
2. 若两个 `person` 的 `age` 相同, 则按照 `name` 的字典序(`strcmp`)排列.

下面给出对应的 `cmp` 函数:

```c
int person_cmp(const void *n1, const void *n2) {
    person *p1 = (person *) n1;
    person *p2 = (person *) n2;
    if (p1->age > p2->age) {
        return -1;
    } else if (p1->age < p2->age) {
        return 1;
    } else {
        return strcmp(p1->name, p2->name);
    }
}
```

### `qsort` 的高扩展性

正是由于有了 `cmp` 函数, 我们可以**按照所需进行定义"大小"关系, 实现不同场景的排序效果**.

此外, 前三个参数扩展了 `qsort` 的排序元素类型, 可以**适用于任意类型的数组元素**.

## `qsort` 中 `cmp` 的原理

### `cmp` 原理

下面以简单的 `int` 型数组冒泡排序为例, 引入 `cmp` 参数, 完成排序"大小"关系的**扩展**.

下面是仅用于从小到大排列 `int` 元素的冒泡排序:

```c
void bubbleSort(int array[], int len) {
    int i, j, flag;
    for (i = 0; i < len; ++i) {
        flag = 1;
        for (j = 0; j < len - i - 1; ++j) {
            if (array[j] > array[j + 1]) { // swap
                int temp = array[j];
                array[j] = array[j + 1];
                array[j + 1] = temp;
                flag = 0;
            }
        }
        if (flag) {
            break;
        }
    }
}
```

可以注意到第 `6` 行就是排序的关键, 比较两元素的大小. 如果我们是该函数的编写者, 当然希望使用该函数的程序员能够按照自己的意愿来定义元素之间的大小关系. 这里就可以引入外界定义的比较函数完成比较:

```c
void bubbleSort(int array[], int len, int (*cmp)(const int*, const int*)) {
    int i, j, flag;
    for (i = 0; i < len; ++i) {
        flag = 1;
        for (j = 0; j < len - i - 1; ++j) {
            if (cmp(&array[j], &array[j + 1]) > 0) { // swap
                int temp = array[j];
                array[j] = array[j + 1];
                array[j + 1] = temp;
                flag = 0;
            }
        }
        if (flag) {
            break;
        }
    }
}
```

此时只需要传入所定义大小关系对应**函数指针**即可完成自定义大小关系的排序. `qsort`的实现也是基于两个元素的比较, `cmp`的作用是类似的.

### 另类的泛型实现

实现泛型, 主流的方法有:
1. 通用指针 `void*`
2. 更高级语言中的泛型模板

这里介绍一种使用宏配置泛型的方法.

```c
#define bubble_sort(T)\
    void T ## BubbleSort(T array[], T len, T (* cmp)(const T*, const T*)) {\
        T i, j, flag;\
        for (i = 0; i < len; ++i) {\
            flag = 1;\
            for (j = 0; j < len - i - 1; ++j) {\
                if (cmp(&array[j], &array[j + 1]) > 0) {\
                    T temp = array[j];\
                    array[j] = array[j + 1];\
                    array[j + 1] = temp;\
                    flag = 0;\
                }\
            }\
            if (flag) {\
                break;\
            }\
        }\
    }
```

> 宏定义中行末的 `\` 用于告知编译器本行与下一行事实上是同一行. 这是为了适应: 宏定义需在同一行内完成.

> 宏定义中函数名称处出现的 `##` 是宏拼接.

之后便可通过如下方法使用:

```c
bubble_sort(int)

int int_decrease(const int *n1, const int *n2) {
    if (*n1 > *n2) {
        return -1;
    } else if (*n1 < *n2) {
        return 1;
    } else {
        return 0;
    }
}

int main() {
    int arr[] = {1, 0, 2, -3, 5, -2, 4};
    intBubbleSort(arr, 7, int_decrease);
    int i;
    for (i = 0; i < 7; ++i) {
        printf("%d ", arr[i]);
    } // => 5 4 2 1 0 -2 -3
    return 0;
}
```