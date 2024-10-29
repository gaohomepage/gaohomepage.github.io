---
title: Objective File
date: 2023-05-21 15:39:51
tags:
    - system
---
# Computer System([Objective File](https://csstormq.github.io/blog/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%B3%BB%E7%BB%9F%E7%AF%87%E4%B9%8B%E9%93%BE%E6%8E%A5%EF%BC%882%EF%BC%89%EF%BC%9A%E7%9B%AE%E6%A0%87%E6%96%87%E4%BB%B6))

目标文件是一个字节序列，有三种类型

1. 可重定位目标文件(.o file)，由汇编器生成`gcc -c`
2. 可执行目标文件(a.out)，由链接器生成`gcc`
3. 可共享目标文件(.so)，动态链接库文件`gcc -fPIC -shared`

ELF header包含一个字段描述目标文件类型

## What is ELF File

ELF保存了机器代码和对应的meta data以及链接器/加载器

1. 节(section)划分不同功能的二进制数据和代码
2. 段(segment)程序加载到内存的ELF文件单位，一个段包括一个以上的section，每个段具有读写/执行等属性

### 指令和数据

指令和数据存储在两个不同的segment，因为它们具有不同的属性，被映射到两个虚拟内存区域，同时提升程序局部性，在单程序多线程可以节省物理内存(动态链接)

### 符号

链接的目的是找到引用符号的定义，本质上是将引用符号字符串替换为具体地址引用

1. 符号(变量/函数名称)
2. 符号值(地址)

### 符号表

符号的meta data存储为ELF文件的一个段`.symtab`，符号表定义了对应的符号名(name)/值(value)/类型(type)和绑定信息(bind)/大小(size)/符号所在的段(Ndx)

![image-20230521151446651](https://s2.loli.net/2023/05/21/RCvTIijrg18zDkX.png)

绑定信息定义了符号的类型

1. LOCAL 局部符号，对目标文件外部不可见
2. GLOBAL 全局符号
3. WEAK 弱引用

如果符号定义在目标文件中，符号所在段表示段在段表中下标，对于特殊符号，它的Ndx定义为

1. ABS 表示符号表示绝对的值(文件名)
2. COM 未初始化的全局符号
3. UND 符号未定义(外部引用)

符号值表示函数/变量的定义(地址)，根据符号类型含义是

1. 目标文件中，符号不是`COMMON`类型，表示在段中偏移
2. 目标文件中，符号是`COMMON`类型，表示对齐属性
3. 可执行文件中，符号表示虚拟地址

## 可重定位目标文件格式

可以用`objdump -d --section .sectionname filename`打印段信息/或者`readelf -p .sectionname filename`

1. ELF header 存储系统信息(字大小，字节顺序，目标文件类型，机器类型)
2. `.text` 编译程序的机器代码
3. `.rodata`制度数据/const全局变量/静态变量
4. `.data`已初始化且非0的静态变量和全局变量
5. `.bss`非初始化/初始化为0的全局变量和静态变量
6. `.sysmtab`符号表，目标文件中**定义和引用**的1全局变量
7. `.shstrtab`字符串表，定义所有section名称（可以看成一个大字符串，字符串中按照\0分开，Section Header Table中Name字段指定了每个段在strtab中的偏移)

## 可执行目标文件格式

可执行目标文件相比可重定位目标文件增加了一下段用于描述动态链接信息

1. `.interp`指向可执行文件需要的动态链接器的路径
2. `.init`定义`_init_函数，被函数初始化代码调用`
3. Section header table藐视目标文件所有section的位置和大小

## Section Header Table

Section Header Table定义在`/usr/include/elf.h`，type表明section类型，包括

1. 0 无效的section
2. 1 section包含程序定义的信息
3. 2 连接器符号表
4. 3 字符串表
