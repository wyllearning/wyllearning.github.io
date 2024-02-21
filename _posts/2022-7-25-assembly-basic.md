---
layout: post
title: x86 汇编基础
description: x86 汇编基础
categories:
  - 逆向编程
---

# x86 汇编基础篇

## 一、进制

### 1.二进制与十六进制

十六进制 : 方便阅读二进制, 由十六个符号组成(可以是任意符号), 逢十六进一

| 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | A    | B    | C    | D    | E    | F    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0000 | 0001 | 0010 | 0011 | 0100 | 0101 | 0110 | 0111 | 1000 | 1001 | 1010 | 1011 | 1100 | 1101 | 1110 | 1111 |

### 2.数据宽度

- 由于受硬件的制约, 数据都有长度限制, 超过容量最大宽度的数据高位会被丢弃
- 计算机仅存储数据, 数据是有符号或无符号取决于怎么解析

| 名字  | 说明 | 大小   |
| ----- | ---- | ------ |
| BYTE  | 字节 | 8 bit  |
| WORD  | 字   | 16 bit |
| DWORD | 双字 | 32 bit |
| QWORD | 四字 | 64 bit |

## 二、寄存器

用于保存运算使用的临时变量

### 1.32 位通用寄存器

由于硬件限制, 大于寄存器宽度的数据, 高位会被丢弃

| 寄存器                                       | 说明                                  | 大小       | 编号 |
| -------------------------------------------- | ------------------------------------- | ---------- | ---- |
| **EAX**(Extended Accumulator Register)       | 累加器, 通常用于运算和返回值          | 0xFFFFFFFF | 0    |
| **ECX**(Extended Count Register)             | 计数, 通常用于位移、循环计数          | 0xFFFFFFFF | 1    |
| **EDX**(Extended Data Register)              | I/O 指针                              | 0xFFFFFFFF | 2    |
| **EBX**(Extended Base Register)              | DS 段的数据指针                       | 0xFFFFFFFF | 3    |
| **ESP**(Extended Stack Pointer Register)     | 堆栈指针                              | 0xFFFFFFFF | 4    |
| **EBP**(Extended Base Pointer Register)      | SS 段的数据指针                       | 0xFFFFFFFF | 5    |
| **ESI**(Extended Source Index Register)      | 字符串操作的源指针; SS 段的数据指针   | 0xFFFFFFFF | 6    |
| **EDI**(Extended Destination Index Register) | 字符串操作的目标指针; ES 段的数据指针 | 0xFFFFFFFF | 7    |

### 2.寄存器标志位

| Flag                      | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| CF(Carry Flag)            | 进位标志(最高位进位), 用于无符号数运算                       |
| PF(Parity Flag)           | 奇偶标志, 操作结果的低 8 位中 1 的个数为偶数时, 值为 1       |
| AF(Auxiliary Carry Flag)  | 辅助进位标志                                                 |
| ZF(Zero Flag)             | 零标志, 运算结果为 0 时, 值为 1                              |
| SF(Sign Flag)             | 符号标志, 即运算结果(二进制)的最高位的值                     |
| TF(Trap Flag)             | 陷阱标志, TF=1 时, CPU 处于单步执行方式(每执行一条指令就自动执行一次 1 号内部中断), 主要用于 Debug 中 |
| DF(Interrupt Enable Flag) | 中断允许标志                                                 |
| DF(Direction Flag)        | 方向标志, 字符串操作时, DF=0, 地址寄存器(SI,DI)的内容递增, DF=1 时, (SI,DI)的内容递减 |
| OF(Overflow Flag)         | 溢出标志, 用于有符号数运算                                   |
| IOPL(I/O Privilege Level) | I/O 特权标志(2 bit), 如果当前特权级别小于等于 IOPL 的值, 则 I/O 指令可执行, 否则将发生一个保持异常 |
| NT(Nested Task)           | 嵌套任务标志, 用于控制中断返回指令 IRET 的执行, NT=0, 执行常规的中断返回操作, 用堆栈中保存的值恢复 EFLAGS、CS 和 EIP; NT=1, 通过任务转换实现中断返回 |
| RF(Restart Flag)          | 重启标志, 用于控制是否接受调试故障, RF=0 表示接受            |
| VM(Virtual 8086 Mode)     | 虚拟 8086 方式标志, VM=1 表示处理器处于虚拟 8086 方式下的工作状态, 否则处理一般保护方式下的工作状态 |

