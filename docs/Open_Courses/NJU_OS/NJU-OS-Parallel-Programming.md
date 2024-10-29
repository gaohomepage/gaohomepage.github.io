---
title: NJU OS Parallel Programming
date: 2023-05-21 12:19:25
tags:
    - NJU OS
    - system
---
# 南京大学操作系统(parallel)

## program from state machine perspective

### 状态机

状态被表示为寄存器保存的值，状态迁移表示寄存器值的变化

#### 宏定义的trick

宏经常写成函数调用的形式，比如

```c
#define ADD(x) (x+1)
```

假设A是一个宏形参，则`#A`是替换为字符串A的形参名，称为字符串化，比如

```c
#define PRINT(n) printf(#n);
PRINT(10); // printf("10");
```

`##`相当于字符串拼接

```c
#define VAR(n) x##n
int VAR(10) = 20; // x10 = 20;
```

### From program perspective(C source code)

1. 状态=heap+stack，执行(函数调用，高级语言)会修改状态
2. 函数调用=申请新的stack frame，设置新PC

### for binary executive file

1. 状态=内存(M)+寄存器(R)
2. 程序执行=修改内存/寄存器值  $(M,R)\to (M^\prime,R^\prime)$

> 1. 状态转移是否是确定的？(随机指令)
> 2. 如何实现停机

3. 特殊指令：syscall，控制权交给OS，无条件修改状态机(读写文件，访问硬件)

## Parallel Programming State Machine Model

多个执行流

1. 维护各自的独立栈帧(context)，保证子状态机之间不可见
2. 共享内存

每个状态并发系统选择一个执行流执行

### API `thread.h`

1. `create(fn)`创建入口为fn的线程，fn是参数为int，返回值为void的函数指针
   1. 更新调用线程栈帧
   2. 创建新栈帧
2. `join()`让调用线程等价其它线程结束，如果存在没有结束的线程，调用线程不会提前终止(状态循环)

### 线程共享全局数据，拥有独立的栈区

并发线程操作共享变量导致结果错误

### 原子性

1. thread-safefy的库函数实现，用`lock-unlock`原语解决并发保护
2. 编译器基于单线程优化(eventual consistency)

例如存在一个变量x的写入序列
$$
x= 1,x=2,x=3
$$
随后读取x，可以将前两个写入优化

### 可见性

> `uniq`命令用于去除一个文本文件中的重复行并统计重复行数量，但是这些**重复行必须是连续的**，需要用sort将其排序

一个线程中对共享变量的操作对另一个并行线程是否可见？

1. 内存屏障，保证屏障前的所有内存操作对于屏障后的指令均可见
2. 汇编语句并非是原子的，每条汇编指令翻译成微指令，可以同时**发射**多条指令
   1. 指令之间执行并非可见
   2. 指令commit到局部存储，再经过任意时间写入memory
   3. 需要更强的语义保证并发指令**真正**执行

例子

```cpp
// thread 1
x = 1;
y_ = y;
// thread 2
y = 1;
x_ = x;
```

初始值`x = y = 0`，最终可能的内存状态包括

1. `y_ = 1,x_ = 1`
2. `y_ =1,x_ = 0 `
3. `y_ = 0,x_ = 1`

一个不可能的状态是`x_=0,y_=0`，但是事实上出现了，这是因为`x=1`**并未写入内存**而是写在了单个core的局部缓存中

## 理解并发程序执行

### 互斥算法

> 保证两个线程不能同时执行一段代码

#### 能否设置indicator变量locked？

比如

```cpp
bool tag;
void lock(){
    tag:
    	if(tag != UNLOCK){
            goto tag;
        }
    tag = LOCKED;
}
```

不能保证比较-上锁的原子性

#### Peterson算法(互斥协议)

1. 共享内存，共享变量x
2. `store(x,v)`写共享内存，`load(x)`加载共享变量值

preliminary:两个竞争者A,B，在共享内存中分别拥有bool变量a,b，为竞争代码段设置一个indicator x，当A/B希望进入竞争段，首先执行

1. 将自身的indicator variable $a/b$设置为`true`
2. 向代码段的indicator x写入B/A

随后进行规则检查(此时共享代码段的indicator已经被写入)，判断

1. 对方的indicator variable `b/a`是否为true
2. x是否**不为**$A/B$

上述两个条件均满足，则**等待**，否则进入代码段

> 线程load一个变量所获得的变量状态本质上是过去某个时间点写入的结果，但是不能保证load之后到上锁的一段时间内锁的状态被改变

写入共享变量x保证：同时改变自身访问状态的两个线程，先写入x的进程进入线程

> 维护一个**私有的共享变量**保证了每个进程拥有自身的状态

##### Example

A先进入临界区，在此之前设置`a=true,X=B`，此时B希望进入临界区，设置`b= true,X = A`，判断

1. $a =true$
2. $X \neq B$

不能进入临界区，等待

若A/B同时准备进入临界区，则$a = b = true$，两个进程同时写X，结果是

> 哪个进程先写入X，哪个进程得以进入临界区

注意条件判断中load(X)必须**真实到X中load**，否则仍然会导致同时进入临界区，即需要保证每个写操作真正写内存，需要设置它的值是volatile

### 正确性

A进入临界区必须至少满足一个条件

1. $b=false$，则B已经从临界区退出
2. $X = A$，则B后写共享变量X，先写入者进入临界区

原子变量保证所有进程对于某一变量在同一时刻的视图都是相同，一个Peterson算法的例子

```cpp
volatile atomic<bool> indicatora(false),indicatorb(false);
enum sharedindicator {INITSTATE,A,B};
volatile atomic<int> shared(INITSTATE);

volatile int sum = 0;
void a(){
    for(int i = 0;i < N;i++){
        indicatora = true;// PC1 =  1
        shared = B;// PC1 = 2
        while((indicatorb == true) && (shared == B)){
            ;// PC1 = 3
        }
        sum += 1;
        indicatora = false;// PC1 = 4
    }
}
void b(){
    for(int i = 0;i < N;i++){
        indicatorb = true; // PC2 = 1
        shared = A;// PC2 = 2
        while((indicatora == true) && (shared == A)){
            ;// PC3 = 3
        }
        sum += 1;
        indicatorb = false; // PC4 = 4
    }
}
```

状态用五元组表示`(indicatora,indicatorb,shared,PC1,PC2)`，初始状态
$$
(false,false,INITSTATE,1,1)\tag{Init State}\label{S0}
$$
选择a执行一步，得到
$$
(true,false,INITSTATE,2,1)\tag{Init State,a}
$$
选择b执行一步，得到
$$
(false,true,INITSTATE,1,2)\tag{Init State,b}
$$

> shared变量和indicator赋值顺序能否交换？处理器乱序？

### yield保存函数状态机

yield返回值同时保存函数状态机，下次调用恢复状态机，继续执行直到遇到下一个yield

