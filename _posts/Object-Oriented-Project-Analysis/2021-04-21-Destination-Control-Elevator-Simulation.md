---	
layout:     post	
title:      『Object-Oriented Project Analysis』 Destination Control Elevator Simulation	
subtitle:   『面向对象项目分析』 目的选层电梯模拟    
date:       2021-04-21	   
author:     Coekjan 
header-img: img/post-bg-OO.jpg	
catalog:    true	
katex:    true    
tags:	
    - Object-Oriented Project Analysis  
---

## 项目背景与描述

**实时交互**的电梯系统, 模拟目的选层电梯在不同应用场景下的调度.

> 为实现控制台中的实时交互, 须使用多线程的方式进行设计.

### 目的选层电梯

**目的选层电梯**, 是指乘客在电梯口前输入其目标楼层. 电梯系统接收到请求后, 可根据电梯当前的状态进行**调度优化**, 实现更高的乘客运送效率.

本项目中电梯运行在1~20层, 初始化时电梯位于1层, 电梯内部没有乘客. 为了模拟方便, 本项目中的电梯的开门速度与关门速度为**0.2s/次**, 乘客在**开门后和关门前的任意时刻**都可以上下电梯. 也就是说, 每次开关门的间隙中, 乘客至少有0.4s的时间上下电梯.

#### 特种电梯

本项目中不同类型的电梯具有不同的特性:

型号 | 可停靠楼层 | 移动速度 | 最大载客量
:-: | :-: | :-: | :-:
A | 1~20 | 0.6s/层 | 8
B | 奇数 | 0.4s/层 | 6
C | 1~3, 18~20 | 0.2s/层 | 4

初始状态中, 有1部A类电梯, 1部B类电梯, 1部C类电梯

> 考虑换乘, 以加快电梯运行效率

### 随机, 早高峰, 晚高峰

本项目中, 考虑了三种应用场景(到达模式):

应用场景 | 记号 | 详细说明
:-: | :-: | :--
随机 | Random | 任何到达情况都有可能出现
早高峰 | Morning | 乘客均在1层出发, 且乘客到达的间隙不超过2s
晚高峰 | Night | 乘客的目标楼层都是1层, 且乘客一次性全部到达

### 输入与输出

本项目中的输入与输出是**异步**的, 需要**至少**两个线程进行处理.

其中输入的第一条指令点明当前的应用场景, 随后是一系列的乘客请求(包含乘客ID, 起始层, 终点层).

输出应为电梯运行日志.

## 架构设计概览

![]({{ '/img/OO-DCEE-Construction.svg' | prepend: site.baseurl}})

下文将具体介绍这一架构.

## 换乘与请求分派

考虑20层楼, 每层楼作为一个结点, 两结点之间的权重为电梯在期间运行的时间, 就可以构建出一20结点带权图(多重边).

由于结点数目不多, 可以采用邻接矩阵的形式存储权值.

在程序运行开始时, 就可以静态分析出每种电梯的带权图:

```java
// ...
for (int i = MIN_FLOOR; i <= MAX_FLOOR; ++i) {
    for (int j = MIN_FLOOR; j <= MAX_FLOOR; ++j) {
        if (/* i & j is reachable */) {
            graph[i][j] = Math.abs(i - j) * speed;
        } else {
            graph[i][j] = /* Infinity */;
        }
    }
}
// ...
```

### Dijkstra最短路

对于输入的一个请求, 可以通过Dijkstra算法搜寻最短路径(时间作为权重). 但是由于电梯到达出发层需要时间, 出发层排队需要时间这两个原因, 需要对静态构建的图进行动态修正.

```java
// forall j is reachable.
realGraph[source][j] += startTime + waitTime;
// ...
```

其中 `startTime` 是**起步代价**, `waitTime` 是**等候代价**.

修正后, 就可以利用Dijkstra算法完成最短路径的搜寻, 其中需要注意的有:
1. 需要使用 `prevElevator` 数组: `prevElevator[i]` 表示到达 `i` 结点最后需要乘坐的电梯;
2. 遇到权重一样的边, 应当选择负载较小的电梯;
3. 换乘电梯时, 需要对权重进行修正: `weight += startTime + weitTime;` .

#### 代价估计

最短路搜寻过程中涉及的代价有:
1. 起步代价;
2. 等候代价.

##### 起步代价

可以利用电梯当前层 `current` , 目标层 `target` 与期望的起步层 `estimate` 之间的大小关系进行简单地估计距离:
1. 若起步层不可达, 则代价无穷大;
2. 若起步层在电梯到达目标层的过程中不可达, 则估计距离为 `Math.abs(current - target) + Math.abs(target - estimate)` ;
3. 若起步层在电梯到达目标层的过程中可达, 则估计距离为 `Math.abs(current - estimate)` .

随后利用距离乘上速度, 再加上开关门消耗的时间, 即可完成起步代价的估计.

##### 等候代价

等候代价与电梯的最大载客量, 电梯的速度相关:
1. 若当前队列长度小于最大载客量, 则等候代价为 `0`;
2. 否则将当前队列长度加一, 模最大载客量, 得 `m` , 则等候代价为 `penalties * m`.

其中 `penalties` 是一个参数, 与电梯的速度相关, 笔者这里采用了比较激进的惩罚:

```java
penalties = speed * coe;
```

其中 `coe` 系数的最优取值可能可以通过数学手段, 深度学习等方式得到, 笔者这里将其定为与电梯类型相关的值.

### 根据最短路生成请求序列

遍历最短路, 可以得到被拆分后的请求序列与电梯序列, 如从4层出发到18层的请求可能可以拆分如下:

![]({{ '/img/OO-DCEE-Request-Split.svg' | prepend: site.baseurl}})

将这一链表封装为一个类 `PersonRequestStream` , 可以方便后续操作.

## 线程安全控制

多线程编程中, 线程安全控制是最重要, 最需要考究的问题之一. 本项目中, 笔者采用了线程安全类来保证线程安全.

### 线程安全类

本项目中, 笔者设计了 `PersonQueue` 线程安全类, 连接生产者 `InstructionListener & Dispatcher` 和消费者 `Elevator` . **值得注意的是, 线程安全类需要具备哪些方法, 应当是根据生产者与消费者的需求来确定的**.

采用线程安全类, 可以极大减轻线程安全设计的工作量, 尽可能让所有的线程安全控制(如 `synchronized` )集中在一个类中.

### 线程结束方法

本项目中, 笔者采用信号传递的方式结束线程.

$$
\begin{CD}
    \mathtt{InstructionListener} @>\mathtt{end}>> \mathtt{Dispatcher} \\
    @. @V\mathtt{end}VV \\
    @. \mathtt{PersonQueue(s)} @>\mathtt{end}>> \mathtt{Elevator(s)}
\end{CD}
$$

但是考虑到尽管外部输入已经结束, `Elevator` 仍有可能将请求送回 `Dispatcher` 中进行分派(换乘). 故不能仅靠 `InstructionListener` 的 `end` 信号, 就向 `PersonQueue(s)` 发送 `end` 信号.

笔者此处利用了前述 `PersonRequestStream` 来完成信号的正确发送.

```java
// inside Dispatcher
private final List<PersonQueue> personQueues = new ArrayList<>();
private final Set<PersonRequestStream> requestSet = new HashSet<>();

public void end() throws InterruptedException {
    while (true) {
        synchronized (requestSet) {
            if (requestSet.stream().anyMatch(PersonRequestStream::hasNext)) {
                requestSet.wait();
            } else {
                break;
            }
        }
    }
    for (PersonQueue queue : personQueues) {
        queue.end();
    }
}
```

只要 `Dispatcher` 从 `Elevator` 接收到重分配的请求时, 就告知 `requestSet` 的同步锁, 该循环将被唤醒. 直至所有的请求均完成( `hasNext` 均返回 `false` ), 就向所有电梯发送结束信号.

## 策略模式与状态模式

### 针对不同应用场景的策略

策略是指考虑根据电梯当前的运行情况, 电梯外部与内部队列, 给出电梯的目的地. 

而对于不同的应用场景, 策略往往是有差异的. 这时候可以使用策略模式进行设计, 将策略算法封装为一个类.

```java
public abstract class Strategy {
    public static final int EXIT = -1;
    private final PersonQueue outside; // persons waiting elevator
    private final List<PersonRequestStream> inside; // persons inside elevator
    // ...
    public abstract int decideTarget();
    // ...
    protected PersonQueue outside() {
        return outside;
    }

    protected List<PersonRequestStream> inside() {
        return inside;
    }
}
```

若要实现ALS调度:
1. 若电梯中无乘客, 则去接收外部队列中最早到达的乘客;
2. 若电梯种有乘客, 则去释放内部队列中最早到达的乘客.

只需要编写如下的策略:

```java
public class StandardStrategy extends Strategy {
    // ALS
    @Override
    public int decideTarget() {
        if (inside().isEmpty()) {
            PersonRequest first = outside().firstPersonRequest();
            if (first == PersonQueue.EXIT) {
                return EXIT;
            } else {
                return first.getFromFloor();
            }
        } else {
            return inside().get(0).current().getToFloor();
        }
    }
}
```

> 其中 `PersonQueue` 的 `firstPersonRequest` 方法在外部输入完成后, 返回 `PersonQueue.EXIT` 信号, 方便电梯线程的结束. 这一点在后文的线程结束方法将有更详细的介绍.

### 作为有限状态机的电梯

注意到电梯的状态是有限的: 运行, 释放与接受请求, 等待指令. 则可以使用状态模式进行设计, 构造有限状态机.

> 此处使用内部类, 可以避免电梯内部字段被暴露.

```java
public class Elevator extends Thread {
    private abstract static class Status {
        public abstract void handle(Elevator elevator, int target);
    }

    private static class Waiting extends Status {
        @Override
        public void handle(Elevator elevator, int target) {
            // ...
        }
    }

    private static class Running extends Status {
        @Override
        public void handle(Elevator elevator, int target) {
            // ...
        }
    }

    private static class Opening extends Status {
        @Override
        public void handle(Elevator elevator, int target) {
            // ...
        }
    }

    private Status status = new Waiting(); // init
    // ...

    @Override
    public void run() {
        while (true) {
            // ...
        }
    }
}
```

为实现捎带的机制, 应考虑在 `Waiting` 状态转移到 `Opening` 状态的条件.

## 定时输入

可采用辅助程序进行定时输入, 方便测试. 这一工作相对简单, 使用简单的C程序就能完成. 以Windows系统下的测试为例:

> 该程序改造自[该作者](https://github.com/dhy2000)的程序

```c
// TimableInput.c
#include <stdio.h>
#include <time.h>
#include <windows.h>

char buffer[1005];
double current;

int main()
{
    double millis;
    while (scanf("[%lf]", &millis) != EOF) {
        millis *= 1000;
        gets(buffer);
        Sleep(millis - current);
        puts(buffer);
        fflush(stdout);
        current = millis;
    }
    return 0;
}
```

将java代码归档为jar文件, 置于同一目录下, 并在该目录下编写 `stdin.txt` 文件, 运行命令行:

```powershell
gcc TimableInput.c -o TimableInput.exe
TimableInput.exe < stdin.txt | java -jar JarFile.jar
```

即可完成定时输入. 还可以将输出结果存至 `stdout.txt` 中方便查看:

```powershell
TimableInput.exe < stdin.txt | java -jar JarFile.jar > stdout.txt
```

> 由于笔者偷懒, 觉着正确性判定挺麻烦的, 所以没有做自动化测试 Orz