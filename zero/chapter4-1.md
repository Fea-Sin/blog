# Hack 机器语言

Hack 是一个基于冯·诺伊曼架构的 16-位计算机，由一个 CPU、两个独立的内存模块即指令内存和数据内存，
以及两个内存映射 I/O 设备（显示器和键盘）组成

## 内存地址空间

有两个不同的地址空间，指令地址空间以下称指令内存，数据地址空间以下称数据内存

CPU 仅能执行存储在指令内存中的程序，指令内存是只读设备，程序通过某种外部方法被加载到指令内存中，比如
可以在预先烧写了必要程序的 ROM 芯片中实现指令内存。要加载新程序，则要替换整个 ROM 芯片

## 寄存器

有两个称为 D 和 A 的 16-位寄存器，这些寄存器能够被算术和逻辑指令显式地操控，比如`A=D-1`, `D=!A`
D 仅用来储存数据值，A 既可以作为数据寄存器也可以作为地址寄存器，也就是说根据指令的上下文含义，A 中的
内容可以被解释为数值或数据存储器中的地址，或作为指令存储器中的地址

身兼重任的 A 寄存器也被用来对指令存储器进行直接访问，与内存访问规则一致。任何 jump 操作总是执行这样的跳转，
即 跳转到`A寄存器指定的指令`。如此以来若要执行操作`goto 35`就要使用一条指令将 A 的值置为 35，然后第二条指令
就直接使用无需描述地址的 goto 命令。这个操作序列使计算机在下一个时钟周期获取在 InstructionMemory[35]位置上的指令

Hack 语言中的 `@value` 命令

如果 sum 代表内存地址 17，那么`@sum` 功能是 `A <- 17`

每个涉及内存地址的操作需要两个 Hack 命令，一个用来确定将要进行操作的内存单元地址，另一个用来描述进行的操作

Hack 语言包含两种指令，一种是地址指令，也称为 A-指令，另一种是计算指令，也称为 C-指令。每种指令都有二进制表示法
和符号表示法

## A-指令

A-指令主要有三种不同的用途

首先，它提供了唯一一种`将常数输入计算机`的方法

其次，通过将目标数据内存单元的地址放入 A 寄存器，来为将来对该内存单元进行操作的 C-指令 提供必要条件

其三，通过将跳转的目的地址放入 A 寄存器来为执行跳转的 C-指令 提供条件

**C 语言**

```c
// 1 到 100 累加
int i = 1;
int sum = 0;

while (i <= 100) {
  sum += i;
  i++;
}
```

**Hack 机器语言**

```
// 1 到 100 累加

@i // i 指代某个内存单元
M=1 // i = 1
@sum  // sum 指代某个内存单元
M=0   // sum = 0

(LOOP)
@i
D=M   // D = i
@100
D=D-A // D=i-100
@END
D;JGT  // if (i-100) > 0 goto END
@i
D=M   // D=i
@sum
M=D+M  // sum = sum + i
@i
M=M+1  // i = i + 1
@LOOP
0;JMP  // goto LOOP
(END)
@END
0;JMP  // 无限循环
```

## C-指令

C-指令是重点，几乎包含要做的所有事情，指令代码的描述可以说是对以下三种问题的回答

- 计算什么

- 将计算后的值存储到什么地方

- 下一步做什么

A-指令与 C-指令一起几乎决定了计算机所有可能的操作

C-指令编码最左边起第一位为 1，接着的两位没有被使用，剩下的位构成了三个域，分别代表指令符号表述的三个部分。
符号指令 dest=comp;jump 的整个语意可以如下表示

comp 域 告诉 ALU 计算什么

dest 域 指明计算后的结果将被存储到什么地方

jump 域 描述了转移条件，即接下来要取出并执行哪一条命令

**computation 规范**

ALU 所执行的是一组固定的函数集，该函数集的功能实现了对寄存器 D、A、M(代表 M[A])的操作，计算函数指令
comp 域由 1 个 a 位和 6 个 c-位域组成

**Destination 规范**

C-指令的 comp 部分计算出来的值能够被存储在不同的目的地址单元，而具体的位置由指令 3-位 dest 域来描述

如果希望计算机将 Memory[7] 的值加上 1，并将结果也存入 D 寄存器

```
0000 0000 0000 0111 // @7
111a 1101 1101 1000 // MD = M + 1
```

**jump 规范**

C-指令的 jump 告诉计算机下一步将执行什么命令，有两种可能性，计算机获取并执行程序中紧接着当前指令的
下一条指令，这是默认的情况，或者它获取并执行程序中位于其他地址的一条指令，对于后者我们假设 A 寄存器
已经装载了跳转的目的地址

jump 操作能否实际执行取决于 jump 域的三个 j 位域和 ALU 的输出值

jump 命令的例子

```
if Memory[3] = 5 then goto 100
else goto 200
```

实现

```
@3
D=M   // D=Memory[3]
@5
D=D-A  // D=D-5
D;JEQ  // if D=0 goto 100
@200
0;JMP
```

`JEQ` if out=0 jump
`JMP` jump

**I/O 指针**

符合 SCREEN 和 KBD 被预定义以表示 RAM 地址 16384(0x4000)和 24576(0x6000)，这两个地址分别是屏幕
和键盘内存映像的基地址

## 输入/输出处理

Hack 平台能够连接两个外部设备：屏幕和键盘，两个设备与计算机平台的交互都是通过内存映像实现的。屏幕上描绘像素
是通过将二进制值写入与屏幕相关的内存段来实现，同样键盘的输入是通过读取与键盘相关的内存单元来实现的。物理 I/O
设备和它们对应的内存映像是通过连续的循环刷新来同步的

## 二进制文件

二进制文件由文本行组成，每行都是一连串的 16 个 0，1 ASCII 字符，其对应一条单独的机器语言指令。文件中的所有行
组成了机器语言程序。当机器语言程序被加载到计算机指令内存中，约定文件中第 n 行所表示的二进制码被存储到指令内存地址为 n
的单元中，程序行数和内存地址计数都是从 0 开始

Hack 机器语言几乎是所有机器语言中最简单的一种，大多数计算机都有更多的指令、更多的数据类型、更多的寄存器、更多的指令格式、
更多的寻址模式。然而任何 Hack 机器语言不支持的功能都可以在软件中实现，比如 Hack 平台的原始机器语言操作不支持乘法和除法，
显然这些操作在任何高级语言里都是需要的，所以我们后面会在操作系统中去实现它们

## 汇编编译器

它是一种程序，负责将以符号形式表达的汇编程序翻译成用二进制代码编写的可执行程序，除此之外，它还负责管理汇编程序中
所有的系统符号和用户定义符号，并在编译过程中以物理内存地址来替换它们
