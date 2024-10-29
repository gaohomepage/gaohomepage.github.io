---
title: NJU OS mutex
date: 2023-05-21 12:15:46
tags:
    - system
    - NJU OS
---
# 南京大学操作系统(mutex)

practical implementation of mutex

## Review

同时读写共享内存导致出现冲突

1. load的值始终是**过去的**值
2. store的结果不能保证下一次load得到store的结果

## Spin Lock

硬件上保证store+load操作是原子的

1. 避免其它线程在某线程读写共享变量时修改共享变量
2. 单一资源竞争(裁判)

### x86原子操作

```assembly
lock add1 $1,(sum)
```

### 借助原子指令控制共享变量访问

设置共享锁，访问共享变量前申请获得锁，只有获得锁的进程才能进入临界区

### 原子指令模型

1. lock对所有线程可见(存在全局可见的lock顺序)
2. 多处理器程序间构造序

#### 实现硬件锁

机器指令层次用一定前缀作为**指令前缀**，译码根据前缀内容向总线发送上锁命令，由总线完成仲裁

> 缓存一致性？如何**控制**其它核的缓存？
>
> > 所有core共享L2 cache，一条lock指令需要删除所有core cache上的对象副本

#### RISC-V实现原子指令

原子指令分成三步(Read-modify-write)

1. 读入内存变量到寄存器中
2. 寄存器执行运算
3. 寄存器的运算写道对应内存位置

> Load-Reserved/Store Conditional(LR/SC)
>
> 对共享变量**打标记(load reserved，load link)**，再读入共享变量到局部寄存器，进行本地计算，随后写入到内存需要满足Store Conditional，即操作的共享变量上仍然有这个标记才能进行写操作
>
> 多个load reseved产生并发写入，导致标记消失，此时需要重新打标记再次写入

> compare-and-swap
>
> 1. 读入内存(`lr w`)
> 2. 判断内存是否等于给定值(`bne`)
> 3. 满足条件，则执行操作(写入新值)`sc,w`
> 4. 判断写入是否成功执行
>
> > 写入未完成，则代表锁拥堵（多个进程并发操作）

### 缺陷

1. 原子操作必须保证指令乱序
2. 多个线程只有一个线程在工作，其他线程在死循环
   1. 分时多线程，抢到锁的线程被切换出去了。。
   2. 多线程并不能提升性能(空等)

> 1. 几乎只有一个进程会在某一时刻
> 2. 避免进程切换(内核程序，避免中断)

### Fast Path和Slow path

1. 只有一个进程竞争锁(Fast Path)，Spin lock的代价只是一条原子指令+判断
2. 多个进程等待(Slow Path)

### OS API实现锁的释放和返回(睡眠锁)

`SYSCALL_lock`获得锁

```c
syscall(SYSCALL_lock,&lck)
```

试图抢占锁`lck`，如果抢占成功进入临界区，**否则将自己切换出去**

`SySCALL_unlock`释放锁

```c
syscall(SYSCALL_unlock,&lck)
```

释放`lck`并**唤醒因等待lck而sleep的进程**

> 需要为每个锁维护一个等待队列，相比Spin lock提高了Fast Path开销(需要进入内核)

### Trade Off

#### 进入临界区

1. 获得锁(单步原子指令，对内存用户态的锁进行fetch)
2. 成功则进入临界区
3. 失败进入系统调用，插入等待队列

#### 退出临界区

所有退出临界区的进程都需要进入系统调用唤醒队列中的等待进程

## Note of awk

awk由模式和操作组成

### 模式

1. 正则表达式
2. 关系表达式
3. 模式匹配表达式 `~`匹配 `!~`不匹配
4. BEGIN pattern END 语句块

### 操作

1. 变量数组赋值
2. 输出命令
3. 内置函数
4. 控制流

例子

```shell
awk 'BEGIN {print "start"} pattern{ commands} END{print "end"}' file
```

脚本在单引号中

工作原理

1. 执行`BEGIN {commands}`语句
2. 文件或标准输入读取一行，执行`pattern{commands}`
3. 执行到输入流末尾，执行`END{commands}`

### 预定义变量

1. $\$ n$当前记录的第n个字段，从1开始
2. $\$ 0$执行过程中当前行文本内容
3. NR记录项

求解第一列所有元素和

```shell
awk 'BEGIN {sum = 0} {sum += $1} END{print sum}'
```

### 传递外部变量

-v选项可以将外部变量传递给awk，例如

```shell
let var=10000
awk -v VARIABLE=$var '{print VARIABLE}'
```

### 正则表达式

1. $.$匹配任意一个字符
2. `a*`匹配任意个a
3. `[^ab]`匹配除了ab以外的任意字符
4. `^`匹配行首
5. $匹配行尾

### 打印文本第i列

```shell
awk '{print $1}' filename
```

比较，满足条件则打印

```shell
awk '{if($5>20) {print $1}}' filename
```

如果第5列大于20则打印对应行的第1列，打印也可以用printf，例如以字符串形式打印第一列

```shell
awk '{printf "%s\n",$1}' filename
```

## C处理命令行参数getopt

函数定义为

```c
int getopt(int argc, char * const argv[], const char *optstring);
```

第三个参数是命令行参数选项，字符代表选项

1. 只有字符没有冒号：纯选项，无需参数
2. 一个冒号：选项必须有参数，可以使用空格分割选项和参数，无参数则解析失败
3. 两个冒号：参数缺省

例如`vha:b:c::`表示支持`-v,-h`选项(无需参数)，支持`-a`选项并带一个参数，支持`-c`选项可以缺省参数

解析参数的结果存在全局变量中，维护四个全局变量

```c
extern char * oprarg;// 存放选项参数字符串
extern int optind; // 下一个检索位置，可以用argv[optind-1]遍历当前解析的命令行参数字段
```

通过while循环遍历所有命令行参数，直到返回-1

### getopt_long()

接收长选项，通过设置一个结构体`struct option `实现，参数是

```c
int getopt_long(int argc,char * const argv[],const char *optsring,const struct option * longopts,int *longindex);
```

1. optstring 用于接收短选项，只接受短选项则将longopts设置为NULL即可(貌似这个选项不能设置为NULL)
2. 长选项个数可以是`--arg=param`或`--arg param`
3. longopts指向struct option数组第一个元素
4. longindex返回longopts数组元素位置指针(有多少个长选项)
5. 参数存在问题返回`?`

option结构体包含

```c
struct option{
    const char *name; // 选项名
    int has_args; // 是否带参数 0-不带参数 1-必须带参数 2-参数可选
    int *flag; // 决定getopt_long返回值类型，flag == NULL一维识别选项后返回val，否则返回0，flag设置为指向val的变量
    int val;// 返回值(长命令的短缩写)
};
```

`longopts`数组最后一个元素需要全0填充

## 内联汇编实现寄存器现场切换

### 内联汇编语法

```assembly
__asm__(assembler template 
	: output operands                  /* optional */
	: input operands                   /* optional */
	: list of clobbered registers      /* optional */
	);
```

1. assemble template 汇编指令
2. output operand 指示汇编指令的计算结果输出到那个C操作数中(左值)
3. input operands 指示汇编指令操作数来自哪些C操作数
4. list of register表示汇编指令修改的寄存器列表

### 实现赋值`x=y`

```assembly
	asm ("movl %1, %%eax; 
              movl %%eax, %0;"
             :"=r"(b)        /* output */
             :"r"(a)         /* input */
             :"%eax"         /* clobbered register */
             );       

```

1. b是输出操作数，对应第二条汇编中movl指令的目的操作数，用%0refer
2. a是输入操作数，用%1refer
3. %%用于确定操作数来自真正的寄存器
4. `=r,r`称为constrain，意思是存到寄存器/从寄存器中load，m代表操作数来自memory

### 实现原子+

```assembly
 __asm__ __volatile__(
                      "   lock       ;\n"
                      "   addl %1,%0 ;\n"
                      : "=m"  (my_var)
                      : "ir"  (my_int), "m" (my_var)
                      :                                 /* no clobber-list */
                      );
```

等效于计算`my_var = my_int+my_var`，`lock `是指令前缀，满足

1. 修饰的指令不能被中断打断，实现需要锁住变量对应的cache line，只有执行这一指令的线程可以存取cache line，其余core需要直接从内存读取(变量存储在单一cache line的情况)
2. 内存屏障，写对象一定刷新到内存

不会改变寄存器值

### 理解线程库中的保护现场

```assembly
    "movq %0, %%rsp; movq %2, %%rdi; jmp *%1"
      : : "b"((uintptr_t)sp), "d"(entry), "a"(arg) : "memory"
```

实现了栈帧切换、传递参数并执行函数调用，指定操作数

1. %0 (uint_ptr_t)sp栈帧指针
2. %1 entry 入口地址
3. %2 args 参数，赋值给%rdi(reserved用于参数传递)

b,d,a制定了操作数加载的寄存器，`jmp *%1`表示跳转到寄存器间接存储的内存位置，x86堆栈需要按照16字节对齐

### Constrain

#### `r`

目标操作数写道寄存器中，寄存器值加载到内存对象中，可以指定使用的寄存器

![image-20230329104904099](https://s2.loli.net/2023/03/29/4IKgXHwGV2bPQei.png)

#### `m`

直接对内存对象操作

#### Digital constraints

数据可能既作为输入也是输出，通过数字指定操作符

## 读文档`setjmp&&longjump`

### Description

函数用于实现nonlocal goto，即控制流从一个函数转移到另一个函数某个位置

`setjmp(jmp_buf)`接收一个`jmp_buf`参数，预定义一个跳转目标，这个函数保存了调用处的运行时状态，包括

1. stack pointer
2. instruction point
3. values of register
4. signal mask

最后一个我理解为程序运行时可能**忽略**某些信号，这也是运行时信息的一部分

`longjmp(jmp_buf,int)`用于将控制权传递给setjmp调用处并恢复调用处运行时环境(restor the stack to its state at the time of the setjmp)

### 返回值

`setjmp`直接调用时返回0，在fake goto的情形返回指定的参数val

## M2

`co_yield`选择线程切换，选择的线程有两种情况

1. 协程新创建，此时需要切换堆栈，再执行代码
2. yield切换出的协程已经通过setjmp保存寄存器，直接调用`longjmp`恢复寄存器现场

### `co_yield`

决定协程何时释放执行流，yield之前需要用`setjmp`保存寄存器现场

### 保护函数堆栈

需要在用户态保存函数堆栈，即再co结构体中维护一个数组

```c
struct co{
    uint8_t stack[STACK_SIZE + 1];
};
```

还需要保存函数指针和参数

```c
void *arg;
void (*func)(void *);
```

维护协程状态，用于`co_wait`等待目标协程执行结束或者唤醒方式

```c
enum co_status status;
```

维护唤醒队列

```c
struct co *waiter;
```

同时需要维护一个current指针，指向当前运行的协程(用于协程切换)
