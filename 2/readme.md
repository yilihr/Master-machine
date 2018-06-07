# MCS-51单片机结构及原理
[TOC]
## 1 MCS-51单片机结构 
#### 1.1 MCS-51单片机的内部结构 
SCM——将通用微计算机基本功能部件集成在一块芯片上构成的一种专用微计算机系统
![51单片机结构组成](https://upload-images.jianshu.io/upload_images/1887348-b4bc2c0140115c00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
SCM = CPU+OSC+ROM+RAM+T/C+INT+ BEC+I/O+UART
80C51=(8位）CPU + 4KBROM + 128BRAM + (2×16)T/C + (4×8)I/O + 1个UART +5个INT + 2个64KB BEC
***
CPU = 控制器 + 运算器
![cpu](https://upload-images.jianshu.io/upload_images/1887348-00d0c8c279f1343b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
控制器
>控制器的用途：统一指挥和控制各单元协调工作
控制器的任务：从ROM中取出指令→译码→执行指令
控制器的组成：程序计数器PC、数据指针寄存器DPTR、指令寄存器IR、指令译码器ID
*PC:指向ROM存储单元的地址指针（16位寄存器）
DPTR:指向ROM或RAM存储单元的地址指针(16位寄存器,可拆为2个8位的独立寄存器DPL和DPH)*

运算器
>运算器的用途：对数据进行算术运算和逻辑操作
运算器的任务：计算缓存器内容→结果暂存→修改运行标志
运算器的组成：累加器ACC、程序状态字寄存器PSW、算术逻辑部件ALU
*ACC:存放操作数或中间运算结果的寄存器
PSW:存放程序运行过程中的各种状态信息的寄存器*

![PSW](https://upload-images.jianshu.io/upload_images/1887348-b8d8c5345c1eee6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
CY（PSW.7）——进位标志.在进行加或减运算时，如果操作结果最高位有进位
或借位时CY由硬件置“1”，否则清“0”。可根据CY判断有无进位或借位；可作
为位操作中的位累加器用。


AC（PSW.6）——辅助进位标志.在进行加或减运算时，如果操作结果的低四位
数向高四位产生进位或借位时，将由硬件置“1”，否则清“0”。根据AC判断加
减运算时有无半进位或半借位；在BCD码调整运算中要用到AC标志


F0（PSW.5）和 F1（PSW.1） ——用户标志位,可做为用户指定的状态标志

RS1（PSW.4）和RS0（PSW.3）——工作寄存器组指针,用于指定CPU的当前工
作寄存器组

OV（PSW.2） ——溢出标志.在有符号数加减运算或无符号数乘除运算中若有
异常结果，OV硬件置1，否则硬件清0。判断运算的结果是否正确，正确 = 0；
出错 = 1

P（PSW.0） ——奇偶标志位.该位始终跟踪累加器A中含“1”个数的奇偶性.如
果A中有奇数个“1”，则P置“1”，否则置“0”.串行通讯中的数据校验，判断是
否存在传输错误
```
#### 1.2 MCS-51引脚及功能 
51系列单片机一般采用40只引脚的双列直插式（DIP——Dual In-line Package）封装结构
除DIP封装外， 51单片机还采用44只引脚的方形扁平(QFP 
——Quad Flat Package) 封装方式（4只引脚无用）。 
![DIP引脚分布](https://upload-images.jianshu.io/upload_images/1887348-6a7cbdb19cbfb9ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

80C51单片机的40只引脚按功能分为以下三类：
* 电源及晶振引脚（共4只)
* 控制引脚（共4只）
* 端口引脚 （共32只） 

***
电源及晶振引脚
>电源引脚：
VCC (40脚)：+5V电源引脚
VSS (20脚)： 接地引脚
外接晶振引脚：
XTAL1 (19脚)；外接晶振引脚（内置放大器输入端）
XTAL2 (18脚)：外接晶振引脚（内置放大器输出端）

控制引脚
>RST/VPD (9)：复位/ 备用电源引脚 
ALE/!(PROG) (30)：地址锁存使能输出/ 编程脉冲输入 
!(PSEN) (29)：输出访问外部ROM读选通信号 
!(EA)/ VPP (31)：外部ROM允许访问/ 编程电源输入;当!(EA)为1，CPU从片内ROM开始读取指令。当PC的值超过4KB时，自动转向片外ROM的指令。当!(EA)为0时，CPU仅访问片外RON

并行I/O口引脚
>P0.0～P0.7（39～32脚）——P0口
P1.0～P1.7（1～8脚）——P1口
P2.0～P2.7（21～28脚）——P2口
P3.0～P3.7（10～17脚）——P3口
8只/组×4 组= 32 只引脚
P0口～P3口是单片机对外联络的重要通道

## 2 MCS-51的存储器结构 
#### 2.1 存储器划分方法
计算机存储器地址空间的两种结构形式：普林斯顿结构和哈佛结构。
|存储结构|编址方式|
|-|-|
|哈佛结构|RAM和ROM分别编址|
|普林斯顿结构|RAM和ROM统一编址 |
51单片机采用哈佛结构，共有4个物理存储空间：
片内RAM、片内ROM、片外RAM、片外ROM
`程序存储器ROM` `数据存储器RAM`
![MCS-51单片机存储结构](https://upload-images.jianshu.io/upload_images/1887348-f7232cad11c79bd5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 2.2 程序存储器
#### 2.3 数据存储器
## 3 单片机的复位、时钟与时序 
#### 3.1 复位与复位电路
#### 3.2 时钟电路
#### 3.3 单片机时序
## 4 并行I/O口 
#### 4.1 P1口
#### 4.2 P3口
#### 4.3 P0口
#### 4.4 P2口
