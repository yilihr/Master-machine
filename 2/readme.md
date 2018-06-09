* [MCS\-51单片机结构及原理](#mcs-51%E5%8D%95%E7%89%87%E6%9C%BA%E7%BB%93%E6%9E%84%E5%8F%8A%E5%8E%9F%E7%90%86)
  * [1 MCS\-51单片机结构](#1-mcs-51%E5%8D%95%E7%89%87%E6%9C%BA%E7%BB%93%E6%9E%84)
      * [1\.1 MCS\-51单片机的内部结构](#11-mcs-51%E5%8D%95%E7%89%87%E6%9C%BA%E7%9A%84%E5%86%85%E9%83%A8%E7%BB%93%E6%9E%84)
      * [1\.2 MCS\-51引脚及功能](#12-mcs-51%E5%BC%95%E8%84%9A%E5%8F%8A%E5%8A%9F%E8%83%BD)
  * [2 MCS\-51的存储器结构](#2-mcs-51%E7%9A%84%E5%AD%98%E5%82%A8%E5%99%A8%E7%BB%93%E6%9E%84)
      * [2\.1 存储器划分方法](#21-%E5%AD%98%E5%82%A8%E5%99%A8%E5%88%92%E5%88%86%E6%96%B9%E6%B3%95)
      * [2\.2 程序存储器（ROM）](#22-%E7%A8%8B%E5%BA%8F%E5%AD%98%E5%82%A8%E5%99%A8rom)
      * [2\.3 数据存储器(RAM)](#23-%E6%95%B0%E6%8D%AE%E5%AD%98%E5%82%A8%E5%99%A8ram)
  * [3 单片机的复位、时钟与时序](#3-%E5%8D%95%E7%89%87%E6%9C%BA%E7%9A%84%E5%A4%8D%E4%BD%8D%E6%97%B6%E9%92%9F%E4%B8%8E%E6%97%B6%E5%BA%8F)
      * [3\.1 复位与复位电路](#31-%E5%A4%8D%E4%BD%8D%E4%B8%8E%E5%A4%8D%E4%BD%8D%E7%94%B5%E8%B7%AF)
      * [3\.2 时钟电路](#32-%E6%97%B6%E9%92%9F%E7%94%B5%E8%B7%AF)
      * [3\.3 单片机时序](#33-%E5%8D%95%E7%89%87%E6%9C%BA%E6%97%B6%E5%BA%8F)
  * [4 并行I/O口](#4-%E5%B9%B6%E8%A1%8Cio%E5%8F%A3)
      * [4\.1 P1口](#41-p1%E5%8F%A3)
      * [4\.2 P3口](#42-p3%E5%8F%A3)
      * [4\.3 P0口](#43-p0%E5%8F%A3)
      * [4\.4 P2口](#44-p2%E5%8F%A3)
      * [P0～P3小结](#p0p3%E5%B0%8F%E7%BB%93)
  * [小结](#%E5%B0%8F%E7%BB%93)
# MCS-51单片机结构及原理
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

51单片机的四个物理存储空间仅相当于三个逻辑存储空间

#### 2.2 程序存储器（ROM）

作用：存放程序、表格或常数，具有非易失性
特点：片内ROM与片外ROM可有2种组合方案
由!(EA)引脚的电平状态决定对片内，片外两种ROM的选择：
>EA=1时为方案1， EA=0时为方案2
方案1 ： 4 KB以内的地址在片内ROM，大于4KB的地址在片外ROM中（图中折线），两者共同构成64KB空间；
方案2 ：片内ROM被禁用，全部64KB地址都在片外ROM中（图中直线）。

![ROM](https://upload-images.jianshu.io/upload_images/1887348-b0c2ed83a6448479.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

由于片内、外ROM是统一编址的，故只能算作1个逻辑存储空间。


ROM有6个特殊存储器单元——用于程序引导
>   0000H：主程序入口地址
    0003H：INT0中断程序入口地址
    000BH：T0中断程序入口地址
    0013H：INT1中断程序入口地址
    001BH：T1中断程序入口地址
    0023H：RI/TI中断程序入口地址
中断程序执行过程：某一突发事件→相应中断入口地址自动装入PC→引导两次跳转→执行相应中断服务程序主程序一般应安排在0030H地址以后（有中断需要时）

#### 2.3 数据存储器(RAM)
数据存储器用于存放运算中间结果、标志位、待调试的程序等。数据存储器由RAM构成，一旦掉电，其数据将丢失。
数据存储器在物理上和逻辑上都占有两个地址空间：一个是片内256B的RAM，另一个是片外最大可扩充的64KB的RAM。

![片内RAM配置](https://upload-images.jianshu.io/upload_images/1887348-c706d601af76435b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

片内RAM分为高128B、低128B两部分，其中，低128B（ 00H～7FH ）为普通RAM区，高128B （80H～FFH）为特殊功能寄存器区

* 低128BRAM区
    此区又可分为三个区：①工作寄存器区（00H～1FH）②可位寻址区（20H～2FH）③用户RAM区（30H～7FH）
    **注意：①区和③区只能按字节进行数据存取操作，②区则可按字节和位两种方式存取操作**
    
    ①工作寄存器区（00H～1FH） 
    >共有32个存储单元；
    每个单元都有1个8位地址（字节地址）
    每个单元都有1个寄存器名称（R0～R7）
    32个单元分为4组（第0 ～ 第3组）
    CPU只能选一组为当前工作寄存器组
    当前工作寄存器组取决于PSW的设置 
    CPU复位后RS1和 RS0默认值为0，即默认第0组为当前工作寄存器组。 

    ![工作寄存器的地址分配表](https://upload-images.jianshu.io/upload_images/1887348-a41e9586ac9cdd7f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

	②可位寻址区（20H～2FH）
    >共有16个存储单元；
    每个单元都有一个字节地址
    每个单元都有8个不同的位地址
    共有128个位地址 
    可以字节地址和位地址两种方式存取数据

    ![位寻址与地址寻址](https://upload-images.jianshu.io/upload_images/1887348-e0f3f18803954a7f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    ③用户RAM区（30H～7FH）
    >共有80个存储单元；
    每个单元都有一个字节地址，但没有位地址，也没有寄存器名。
    此区可作为堆栈区和中间数据存储区使用——用户RAM区

* 高128BRAM区
每个存储单元都有一个字节地址，但只有其中21个单元可以使用，并有相应寄存器名称。
51单片机共有21个特殊功能寄存器（SFR）：

![SFR的名称及其分布](https://upload-images.jianshu.io/upload_images/1887348-189b85d21d7f7553.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3 单片机的复位、时钟与时序 
#### 3.1 复位与复位电路
>单片机在开机时需要复位，使CPU及其他功能部件处于一个确定的初始状态；另外在单片机死机时，也需要复位。
复位——使单片机恢复原始默认状态的操作。

![复位时片内各寄存器的初始值](https://upload-images.jianshu.io/upload_images/1887348-a6434c098e8b67f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

复位条件:在RST/VPD引脚端出现：≥10ms时间的高电平（≥3V）状态
复位方式：

![复位方式](https://upload-images.jianshu.io/upload_images/1887348-b465e9d2e2a29438.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3.2 时钟电路
单片机执行指令的过程可以分为取指令， 分析指令和执行指令三个步骤，每个步骤又由许多微操作组成，这些微操作必须在一个统一的时钟控制下才能按照正确的顺序执行。

单片机需要统一的时钟控制，其时钟系统可有两种方案：
内部OSC + 外部时钟电路，或内部OSC + 外部时钟脉冲

![外部时钟电路](https://upload-images.jianshu.io/upload_images/1887348-096ac3d20b2a3514.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![外部时钟脉冲](https://upload-images.jianshu.io/upload_images/1887348-8f6a5851a2a75457.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

MCS-51的时钟频率一般为6～12MHz

#### 3.3 单片机时序
* 时序的概念
时序是对象（或引脚、事件、信息）间按照时间顺序组成的序列关系。
时序可以用状态方程、状态图、状态表和时序图4种方法表示，其中时序图最为常用。
时序图亦称为波形图或序列图，纵坐标表示不同对象的电平，横坐标表示时间（从左往右为时间正向轴），通常坐标轴可省略。
    >以下图为例：
(1) 最左边是引脚的标识，表示该图反映了RS、R/W、E、D0-D7四类引脚的序列关系。 
(2) 交叉线部分表示电平的变化，如高电平和低电平。 
(3) 封闭菱形部分表示数据有效范围（偶尔使用文字Valid Data）。
(4) 水平方向的尺寸线表示持续时间的长度。

![](https://upload-images.jianshu.io/upload_images/1887348-6224e6d68fbf2b97.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    时序关系：
RS和R/W端首先变为低电平；
随后D0~D7端出现有效数据；
R/W低电平tsp1之后，E端出现宽度为tpm的正脉冲；
E脉冲结束并延时tHD1后，RS和R/W端恢复高电平；
E脉冲结束并延时tHD2后，D0~D7端的本次数据结束；
随后D0~D7端出现新的数据，但下次E脉冲应在tc时间后才能出现。根据这些信息便可以进行相应的软件编程了。

* 时钟的度量单位：
时钟周期（或节拍）P、状态周期S、机器周期、指令周期
    >MCS-51各种周期之间的关系

![MCS-51各种周期之间的关系](https://upload-images.jianshu.io/upload_images/1887348-489f4e00adceedbe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1个状态周期（S）= 2个节拍（P）
1个机器周期 = 6个状态（S） =12个节拍（P）
1个指令周期约为1～4个机器周期


* 时序与时钟的关系——时序要受时钟节拍的制约
    >`ADC0809芯片的完整时序图`

![ADC0809芯片的完整时序图](https://upload-images.jianshu.io/upload_images/1887348-07f163aaf309ee4a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 单片机时序——CPU在执行指令时所需控制信号的时间顺序
CPU实质上就是一个复杂的同步时序电路，这个时序电路是在时钟信号的推动下工作的。在执行指令时，CPU首先要到存储器中取出需要执行指令的指令码，然后对指令进行译码，并由时序部件产生一系列控制信号去完成指令的执行。
从用途来看，CPU发出的时序信号可以分为两类：
1、用于片内各功能部件的逻辑控制
2、用于片外RAM访问或总线方式控制

单片机常用时序逻辑元件——D触发器（或边沿D触发器）
D触发器可以分为正边沿D触发器和负边沿D触发器
>正边D沿触发器
正边沿D触发器的原理：

![正边沿D触发器的原理](https://upload-images.jianshu.io/upload_images/1887348-32a21f11f9e8ae4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

只在时钟脉冲CLK上升沿到来的时刻，才采样D端的输入信号，并据此立即改变Q和/Q端的输出状态。而在其它时刻，D与Q是信号隔离的。 

>负边沿D触发器
负边沿D触发器的原理

![负边沿D触发器的原理](https://upload-images.jianshu.io/upload_images/1887348-5d5159d068acb913.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

负边沿D触发器工作特性
只在时钟脉冲CLK下降沿到来的时刻，才采样D端的输入信号，并据此立即改变Q和/Q端的输出状态。而在其它时刻，D与Q是信号隔离的。
D触发器的这一特性被广泛用于数字信号的锁存输出。



## 4 并行I/O口 
51单片机有32只I/O引脚，分属于4个端口（P0～P3）。

可作为并行I/O输入通道（例如，按键/开关连接通道）

![按键/开关连接通道](https://upload-images.jianshu.io/upload_images/1887348-7af8f789fcda5829.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可作为并行I/O输出通道（例如，数码管显示器连接通道）

![数码管显示器连接通道](https://upload-images.jianshu.io/upload_images/1887348-d69bcbd6df6e83ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可作为串行通信通道（例如，双机通讯的连接通道）

![双机通讯的连接通道](https://upload-images.jianshu.io/upload_images/1887348-09c78ab685882393.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可作为外部设备的连接通道（例如，存储器扩展通道）

![存储器扩展通道](https://upload-images.jianshu.io/upload_images/1887348-b369c9bbc80c6ec2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 4.1 P1口
P1口包含P1.0～P1.7共8个相同结构的电路
* P1口结构图：
![P1口结构图](https://upload-images.jianshu.io/upload_images/1887348-a4755ea27bf10223.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

P1.n = 1个锁存器 + 1个场效应管驱动器V + 2个三态门缓冲器
P1.0～P1.7中的8个锁存器共同组成P1特殊功能寄存器(90H)

* P1.n的通用I/O口工作方式：
    1. 输出方式；MOV P1,#data
    2. 读引脚方式；MOV A,P1
    3. 读锁存器方式；ANL P1,A
* 要点
    1. P1口具有通用I/O口方式，可实现输出、读引脚（输入）和读锁存器三种功能；
    2. P1口为准双向通用口，作为通用输入口时应先使P1.n→1，作为通用输出口时是无条件的。


#### 4.2 P3口
与P1.n 差别：第二功能控制单元→双功能 

* P3口结构图

![P3口结构图](https://upload-images.jianshu.io/upload_images/1887348-c0dc4c37263c6937.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* P3.n的通用I/O口工作方式：
    1. 输出；
    2. 读引脚；
    3. 读锁存器；
* 要点
    1. P3口具有通用I/O口方式，可实现输出、读引脚（输入）和读锁存器三种功能；
    2. P3口为准双向通用口，作为通用输入口时应先使P3.n→1，作为通用输出口时应先使第二输出端→1 ；
    3. P3口具有第二功能方式，可实现第二输出和第二输入两种功能。

#### 4.3 P0口
与P1.n 差别：输出控制电路、 输出驱动电路→总线功能 

* P0口结构图

![P0口结构图](https://upload-images.jianshu.io/upload_images/1887348-ea5720a008d4b1a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* P0.n的通用I/O口工作方式：
    1. 输出；
    2. 读引脚；
    3. 读锁存器；
* P0.n的地址/数据分时复用方式：
    1. 地址/数据输出
    2. 数据输入

* 要点
    1. P0口具有通用I/O口方式，可实现输出、读引脚（输入）和读锁存器三种功能；
    2. P0口为准双向通用口，作为通用输入口时应先使P3.n→1，作为通用输出口时应先使第二输出端→1 ；
    3. 作为通用I/O口方式时，需要外接上拉电阻；
    4. P0口具有地址/数据分时复用方式，可实现地址/数据输出、数据输入两种功能；
    5. 地址/数据分时复用方式时无需外接上拉电阻；
    6. 分时复用方式的数据输入时无需程序写1操作。

#### 4.4 P2口
与P1.n差别：输出控制单元，锁存信号由Q端输出

* P2口结构图

![P2口结构图](https://upload-images.jianshu.io/upload_images/1887348-e7729f082fc709ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* P2.n的通用I/O口工作方式：
    1. 输出；
    2. 读引脚；
    3. 读锁存器；
* 要点
    1. P2口具有通用I/O口方式，可实现输出、读引脚（输入）和读锁存器三种功能；
    2. P2口为准双向通用口，作为通用输入口时应先使P2.n→1，作为通用输出口时应先使控制端→1 ；
    3. 作为通用I/O口方式时，无需外接上拉电阻；
        4. P2口具有地址输出方式，可实现地址输出功能。

#### P0～P3小结
1. 结构

![结构](https://upload-images.jianshu.io/upload_images/1887348-a1ec94e3f421a9e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 功能

![功能](https://upload-images.jianshu.io/upload_images/1887348-99693a3be1d117c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 小结
1. 单片机的CPU由控制器和运算器组成，在时钟电路和复位电路的支持下，按一定的时序工作。单片机的时序信号包括振荡周期、时钟周期、机器周期和指令周期。
2. 51单片机采用哈佛结构存储器，共有3个逻辑存储空间和4个物理存储空间。片内低128字节RAM中包含4个工作寄存器组、128个位地址单元和80个字节地址单元。片内高128字节RAM中离散分布有21个特殊功能寄存器。
3.  P0～P3口都可作为准双向通用I/O口，其中只有P0口需要外接上拉电阻；在需要扩展片外设备时，P2口可作为其地址线接口，P0口可作为其地址线/数据线复用接口，此时它是真正的双向口。 

