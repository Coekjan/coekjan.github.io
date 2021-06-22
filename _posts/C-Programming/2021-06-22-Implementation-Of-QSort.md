---	
layout:     post	
title:      『C Programming』 Implementation Of QSort	
subtitle:   『C程序设计』 快速排序的实现    
date:       2021-06-22	   
author:     Coekjan 
header-img: img/post-bg-CP.jpg	
catalog:    false    
tags:	
    - C Programming  
---

本文仅记录一份`qsort`的粗糙实现, 可以帮助读者理解该库函数的运行原理.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// IMPLEMENTATION OF QSORT //

typedef unsigned char byte; // sizeof(byte) == 1

void my_qsort(
    void *base, size_t numOfEle, size_t sizeOfEle,
    int (*cmp)(const void*, const void*)
);
void _mqsort(
    void *base, void *buf,
    int left, int right,
    size_t sizeOfEle, int (*cmp)(const void*, const void*)
);
void swap(byte *buffer, byte *p, byte *q, size_t size);

void my_qsort(void *base, size_t numOfEle, size_t sizeOfEle,
              int (*cmp)(const void*, const void*)) {
    byte *buffer = (byte *) malloc(sizeOfEle);
    _mqsort(base, buffer, 0, numOfEle - 1, sizeOfEle, cmp);
    free(buffer);
}
void _mqsort(void *base, void *buf, int left, int right, size_t sizeOfEle,
             int (*cmp)(const void*, const void*)) {
    int i, j;
    byte *pivot;
    if (left < right) {
        i = left;
        j = right + 1;
        pivot = (byte *) base + left * sizeOfEle;
        while (1) {
            while (cmp((byte *) base + (++i) * sizeOfEle, pivot) < 0 &&
                    i != right) {}
            while (cmp((byte *) base + (--j) * sizeOfEle, pivot) > 0 &&
                    j != left) {}
            if (i < j) {
                swap(buf,
                     (byte *) base + i * sizeOfEle,
                     (byte *) base + j * sizeOfEle, sizeOfEle);
            } else break;
        }
        swap(buf,
             (byte *) base + left * sizeOfEle,
             (byte *) base + j * sizeOfEle, sizeOfEle);
        _mqsort(base, buf, left, j - 1, sizeOfEle, cmp);
        _mqsort(base, buf, j + 1, right, sizeOfEle, cmp);
    }
}

void swap(byte *buffer, byte *p, byte *q, size_t size) {
    memcpy(buffer, p, size);
    memcpy(p, q, size);
    memcpy(q, buffer, size);
}

// IMPLEMENTATION OF QSORT //

// ----- TEST - BEGIN ----- //

typedef struct {
    char name[128];
    unsigned number;
} student;

int int_increase(const void *p, const void *q) {
    int *a = (int *) p;
    int *b = (int *) q;
    if (*a > *b) return 1;
    else if (*a < *b) return -1;
    return 0;
}

int stu_increase(const void *p, const void *q) {
    student *s1 = (student *) p;
    student *s2 = (student *) q;
    int cmp_res = strcmp(s1->name, s2->name);
    if (cmp_res != 0) return cmp_res;
    else {
        if (s1->number > s2->number) return 1;
        else if (s1->number < s2->number) return -1;
        return 0;
    }
}

int main() {
    int i, len;
    int num[] = {2, 4, -2, 3, -3, 5, 6, 1};
    len = sizeof(num) / sizeof(int);

    my_qsort(num, len, sizeof(int), int_increase);
    
    for (i = 0; i < len; ++i) printf("%d ", num[i]);
    putchar('\n');
    putchar('\n');

    student stu[] = {
        {"Jack", 30373356},
        {"Mike", 30373333},
        {"Jane", 30373184},
        {"Jack", 30373354},
        {"Olim", 30373366},
        {"Alan", 30373444}
    };
    len = sizeof(stu) / sizeof(student);

    my_qsort(stu, len, sizeof(student), stu_increase);

    for (i = 0; i < len; ++i)
        printf("name [%s] id [%d]\n", stu[i].name, stu[i].number);

    return 0;
}
// ----- TEST - END ----- //
```

控制台输出:

```text
-3 -2 1 2 3 4 5 6

name [Alan] id [30373444]
name [Jack] id [30373354]
name [Jack] id [30373356]
name [Jane] id [30373184]
name [Mike] id [30373333]
name [Olim] id [30373366]
```