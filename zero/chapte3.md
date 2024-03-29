# 时序逻辑

## 触发器(Flip-Flops)

计算机里最基本的时序单元是称为触发器的设备，它有几个变体，我们使用的是**数据触发器**Date Flip Flop，
DFF 或称为 D 触发器，其接口包哈 1 比特位输入和 1 比特位输出。DFF 有个时钟输入，根据主时钟信号连续
地地交变。数据和时钟的输入使得 DFF 能够实现基于时间的行为`out(t)=in(t-1)`，这里 in 和 out 是门的输入
和输出值，t 是当前时钟周期。DFF 简单地将前一个时间周期的输入值作为当前周期的输出

## 寄存器(Registers)

寄存器是具有记忆功能的设备，它能够存储或称记忆某一时刻的值，实现经典的存储行为`out(t)=out(t-1)`。DFF 仅能够
输出它前一时钟周期的输入，这就告诉我们，可以通过 DFF 来实现寄存器，只需要后面的输出反馈到它的输入就可以了

## 计数器(Counter)

计数器是一种时序芯片，它的状态是整数，每经过一个时间周期，该整数就增加 1 单位，执行函数`out(t)=out(t-1)+c`，
计数器在数字系统中担当非常重要的角色。比如，经典的 CPU 包括一个程序计数器，它的输出就是当前程序中下一步将要
执行的指令地址

计数器芯片可以通过将标准寄存器的输入/输出逻辑和附加的常数组合逻辑相结合来实现。一般来说，计数器必须要配置一些
附加的功能块，比如将计数置零、加载新的计数值，或者用减操作来取代增操作

## D 触发器

DFF 门，也是用于设计所有记忆单元的基本组件，DFF 门包含 1 个比特位数据输入和 1 比特输出

跟 Nand 门一样，DFF 门在我们的计算体系中处于非常底层，可以说计算机中的所有时序芯片(寄存球、内存、计数器)
都是基于大量的 DFF 门。所有这些 DFF 门都连接同一个主时钟，形成巨大的分布式`合唱阵容`，计算机系统中的
所有时序芯片都在主时钟频率这个`总指挥`的协调统一之下共同工作

在每个时钟周期的起始点，计算机中的所有 DFF 的输出都被赋予它们上个时钟周期的输入，在其他时间 DFF 被锁存了，
这意味者它们的输入将暂时不会影响它们的输出。组成系统的上百万个 DFF 门，大约每秒十亿次次，次数衣据计算机的
时钟频率而定

通过同时向系统所有 DFF 提供统一的主时钟信号，来实现计算机硬件对时间的相关性，硬件仿真器能够以软件的方式模拟
出相同的效果

只要所有设计的芯片中包含 DFF 门，那么整个芯片乃至在该硬件层之上构建的其他所有芯片都将继续对时间的相关性，
按照定义，这些芯片都称为时序芯片(sequential chip)

> DFF 的物理实现是个复杂的任务，它使用反馈回路来连接几个基本的逻辑门

## 存储

可直接访问的记忆单元称为 RAM，是由 n 个 w 位寄存器组成的阵列，配有直接访问电路。寄存器的个数 n 和每个寄存器宽度 w 分别
称为内存的尺寸和宽度

RAM 读：要读取编号为 m 的寄存器，我们先将 m 置于 address 输入管脚。RAM 的直接存取逻辑将选中编号为 m 的寄存器，该寄存器
于是将它的输出发送到 RAM 的输出管脚。这是个不涉及时间的组合逻辑操作

RAM 写：要将新的数据 d 写入编号为 m 的寄存器，我们同样将 m 置于 address 输入管脚，并将 d 置于 in 输入管脚，然后确认 load 位
为 1。这样使得 RAM 的直接存取逻辑去选中 m 号寄存器，在下一个时钟周期里，被选中的寄存器会被赋予新的数据值 d

## 计数器

虽然计数器是对立的抽象，但是可以通过它的一些应用来描述。比如一个指令地址计数器，它的内容是计算机下一个时钟中期要读取和
执行的指令地址，在大多数情况下，计数器在每个时钟周期内做简单的加 1 操作，因此计算机能够获取程序的下一条指令。其他情况下，
比如要跳跃去执行编号为 n 的指令时，我们希望能够将计数器设为 n，然后继续执行默认的计数行为 n+1,n+2 等等。甚至将计数器
清零。在任何时候可通过将计数器置为 0 来让程序重新执行。我们需要一个可重载的、可重置的计数器

如此一来，我们的计数器芯片接口就与寄存器的接口十分相似，只是计数器芯片有两个附加的控制位 reset 和 inc，当 inc=1 时，
计数器在每个时钟周期自加。如果想要计数器重置为 0，就将 reset 位置为 1，如果想要将其初始化为某个计数值 d，就将 d 置于
in 输入管脚然后 load 位置 1

时序芯片被 DFF 门赋予了维持状态(内存单元)或者对状态进行操作(如计数器)的能力

## 组合逻辑芯片

从计数角度来说，`时序逻辑芯片`内部建立反馈回路来实现的，在组合逻辑芯片中根本没有提到时间

将时序芯片的输出反馈到输入是非常容易的，因为 DFF 中存在的时间延迟，在时刻 t 的输出不依赖于其当前输出值，而依赖于 t-1 时刻的
输出值，这一属性能避免带反馈电路的组合逻辑芯片中出现的数字竞争

组合逻辑芯片的输出随其输入的变化而变化，而不去考虑时间，相比之下，时序结构中包含的 DFF 保证了它们的输出变化仅仅发生在一个时钟
周期到下一个时钟周期的转换点上，而不是在时钟周期之内。实际上允许时序芯片在时钟周期之内出现不稳定的状态，但是必须保证在下一个
时钟周期的起始点，其输出值是正确的

时序芯片输出这种`离散化`过程有个重要作用，它能被用来对整个计算机系统进行同步。例如，假设我们指示算数逻辑单元(ALU)计算 x+y，
其中 x 是于 ALU 邻近的 RAM 寄存器的值，y 是距 ALU 较远 RAM 寄存器的值，由于各种物理条件上的限制(距离、阻力、干涉、随机噪声等等)，
代表 x 和 y 的电信号可能会在不同的时刻达到 ALU。然而对于组合逻辑芯片而言，ALU 并没有时间概念，ALU 只负责不间断地出现在输入端的值
加起来，因此 ALU 的输出稳定到正确的 x+y 结果需要一些时间。在结果稳定之前，ALU 会产生垃圾值

**如何来解决这个问题**

ALU 的输出总是被发送到某种类型的时序芯片(寄存器、RAM 存储单元等等)，只要保证，在设计计算时钟时，时钟周期的长度要比 1 比特
在计算机系统中两个物理距离最长的芯片之间的传输时间稍长。这样就能在时间上保证时序芯片能够更新其状态，也就是保证它从 ALU 哪里接收
到的输入值是有效的。简单的说，就是将一组相互独立的硬件组件同步为一个协调统一的系统

最后，应该说现代计算机的存储器设备并不总是由标准的触发器构建而成，利用内在存储技术的独特物理属性，现代的存储器芯片通常被精心地优化。
对于计算机设计人员而言，有很多可供选择的技术，采用何种技术仍然是个性价比的问题
