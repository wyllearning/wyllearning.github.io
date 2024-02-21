---
layout: post
title: 服务器相关知识点总结
description: 入职新公司新员工知识学习
categories:
  - 服务器
---

# 服务器知识点总结

## 一、服务器通用基础指数

### 1、概念

&emsp;&emsp;服务器就是在网络中为其他客户机提供服务的计算机

### 2、服务器组成

&emsp;&emsp;服务器由CPU、内存、硬盘、RAID卡、网卡组成，需要电源、主板、机箱等基础硬件的配合。

&emsp;&emsp;PS:参考文章：[RAID磁盘阵列与配置](https://blog.csdn.net/weixin_51432770/article/details/110119945?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167204522216800182110308%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167204522216800182110308&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-110119945-null-null.142^v68^pc_new_rank,201^v4^add_ask,213^v2^t3_esquery_v1&utm_term=RAID&spm=1018.2226.3001.4187)

&emsp;&emsp;简称：独立冗余磁盘阵列，将相互独立的物理硬盘按不同的方式组合起来形成一个硬盘组（逻辑硬盘）

&emsp;&emsp;常用的RAID级别：RAID0、RAID1、RAID5、RAID6、RAID1+0等

### 3、服务器主板组成

&emsp;&emsp;PCIe总线、内存、GPU和SSD，

![主板组成图](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E4%B8%BB%E6%9D%BF%E7%BB%84%E6%88%90%E5%9B%BE.png)

### 4、服务器主板总线类型

服务器总线连接图如下：

![服务器总线图](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%80%BB%E7%BA%BF%E5%9B%BE.png)

PCH：芯片组

QPI：四线SPI，即数据线最多可以使用4根

DMI：直接媒体接口，直接媒体接口，是Intel公司开发用于连接主板南北桥的总线。

[参考文章【FSB总线、HT总线、QPI总线、DMI总线】](https://blog.csdn.net/weixin_30379531/article/details/95306571?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167204702716800215084190%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167204702716800215084190&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-95306571-null-null.142^v68^pc_new_rank,201^v4^add_ask,213^v2^t3_esquery_v1&utm_term=DMI%E6%80%BB%E7%BA%BF&spm=1018.2226.3001.4187)

SAS和SATA异同点：SATA标准其实是SAS标准的一个子集，SAS硬盘性能更好，但价格更贵。

[参考文章【SAS和SATA它两的相同点与不同点】](https://blog.csdn.net/weixin_50663202/article/details/109112969?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167204718216800222832289%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167204718216800222832289&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-109112969-null-null.142^v68^pc_new_rank,201^v4^add_ask,213^v2^t3_esquery_v1&utm_term=SAS%E5%92%8CSATA&spm=1018.2226.3001.4187)

PCIE：[【PCIe总线的基础知识】](https://blog.csdn.net/a8039974/article/details/124275174?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167204734316782425116238%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167204734316782425116238&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-1-124275174-null-null.142^v68^pc_new_rank,201^v4^add_ask,213^v2^t3_esquery_v1&utm_term=PCIE%E6%80%BB%E7%BA%BF&spm=1018.2226.3001.4187)

- CPU与CPU之间通过QPI总线进行通信
- CPU与PCI-E设备通过PCIE总线进行通信
- CPU和PCH通过DMI总线进行通信
- PCH芯片和USB设备之间通过USB总线进行通信
- PCH芯片和PCI设备（扩展IO设备）通过PCI总线进行通信
- PCH芯片和PCI-E设备（扩展IO设备）通过PCIE总线进行通信
- PCH芯片和SATA硬盘之间通过SATA总线进行通信
- PCH芯片和SAS硬盘之间通过SAS总线进行通信
- PCH和网卡之间使用PCIE总线进行通信
- BMC与其他设备通过SPI总线进行通信

### 5、ARM中的高速总线和低速总线

&emsp;&emsp;ARM中的总线被为AMBA，它包括三部分AHB、ASB、APB和Test Methodology.

&emsp;&emsp;AHB是超高速总线，ASB是高速总线，APB是低速总线。

&emsp;&emsp;高速总线用于连接CPU,DRAM,DSP等，低速总线用于连接UART,Serial port, SD card等

### 6、服务器PCIe总线

&emsp;&emsp;PCIe总线使用端到端的连接方式，在一条PCIe链路的两端只能各连接一个设备，这两个设备互为数据发送端和数据接收端

![PCIE总线的物理连接](https://kx-image.oss-cn-chengdu.aliyuncs.com/PCIE%E6%80%BB%E7%BA%BF%E7%9A%84%E7%89%A9%E7%90%86%E8%BF%9E%E6%8E%A5.png)

&emsp;&emsp;由上图所示，在PCIe总线的物理链路的一个数据通路(Lane可能有多个数据通路)中，由两组差分信号，共4根信号线组成，目前PCIe链路可以支持1、2、4、8、12、16和<font color=#FF0000 >32</font>个Lane，即×1、×2、×4、×8、×12、×16和<font color=#FF0000 >×32</font> 宽度的 PCIe链路。每一个Lane上使用的总线频率与PCIe总线使用的版本相关

参考文章：[【PCIe总线的基础知识】](https://blog.csdn.net/a8039974/article/details/124275174?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167204734316782425116238%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167204734316782425116238&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-1-124275174-null-null.142^v68^pc_new_rank,201^v4^add_ask,213^v2^t3_esquery_v1&utm_term=PCIE%E6%80%BB%E7%BA%BF&spm=1018.2226.3001.4187)、[【PCIe接口二，三事】](https://blog.csdn.net/IT_Beijing_BIT/article/details/118438515?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167204815816800192238083%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=167204815816800192238083&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~pc_rank_34-2-118438515-null-null.142^v68^pc_new_rank,201^v4^add_ask,213^v2^t3_esquery_v1&utm_term=PCIE%E5%8F%AF%E4%BB%A532%E9%80%9A%E9%81%93%E5%90%97&spm=1018.2226.3001.4187)

***PS：部分文章中描述PCIe可支持到32通道，但是部分文章中仍只支持16通道***

### 7、服务器分类

- 按照CPU指令集分类：Unix服务器、X86服务器，分别为RISC(精简指令集)和CISC(复杂指令集)
- 按处理器数量分类1路、2路、多路以及CPU Cores （Unix服务器）
- 按服务器外形分类：塔式、机架式、刀片式、高密服务器
- 按负载类型分类：数据库服务器、应用服务器 Web服务、接入服务器 文件服务器

### 8、CISC、RISC和VLIW

#### 8.1.CISC（Complex Instruction Set Computer）复杂指令集

- 指令系统复杂庞大，指令数目一般为200条以上
- 指令的长度不固定，指令格式多，寻址方式多
- 可以访存的指令不受限制
- 各种指令使用频度相差很大
- 各种指令执行时间相差很大，大多数指令需多个时钟周期才能完成
- 控制器大多数采用微程序控制
- 难以用优化编译生成高效的目标代码程序

#### 8.2.RISC（Reduced Instruction Set Computer）精简指令集

- 选取使用频率最高的一些简单指令，复杂指令的功能由简单指令的组合来实现

- 指令长度固定，指令格式种类少，寻址方式种类少

- 只有Load/Store(取数/存数)指令访存，其余指令的操作都在寄存器之间进行

- CPU中通用寄存器数量相当多

- RISC一定采用指令流水线技术，大部分指令在一个时钟周期内完成

- 以硬布线控制为主，不用或少用微程序控制

- 特别重视编译优化工作，以减少程序执行时间

  PS：[参考文章【CISC和RISC的比较】](https://blog.csdn.net/weixin_45684362/article/details/124197295?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167204915616782425666683%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167204915616782425666683&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-124197295-null-null.142^v68^pc_new_rank,201^v4^add_ask,213^v2^t3_esquery_v1&utm_term=CISC%E5%92%8CRISC&spm=1018.2226.3001.4187)

#### 8.3.VLIW（Very Long Instruction Word）超长指令集架构

- 每时钟周期可运行20条指令
- 采用了先进的EPIC（清晰并行指令）设计，我们也把这种构架叫做“IA-64架构”
- VLIW要比CISC和RISC强大的多
- 最大优点是简化了处理器的结构，删除了处理器内部许多复杂的控制电路
- VLIW的结构简单，也能够使其芯片制造成本降低，价格低廉，能耗少，而且性能也要高得多
- 目前基于这种指令架构的微处理器主要有Intel的IA-64和AMD的x86-64两种

### 9、服务器扩展插槽

&emsp;&emsp;主要有ISA，PCI，AGP，CNR，AMR，ACR，PCI Express和比较少见的WI-FI，VXB，以及笔记本
电脑用的PCMCIA、Mini PCI等

### 10、服务器散热方式

风冷：①散热片+风扇②均热板+风扇

液冷：①冷板+压缩机②浸没③喷淋

### 11、服务器系统电源

#### 11.1 UPS（UniterruptiblePowerSupply）

&emsp;&emsp;市电输入UPS，经过整流器将交流转换为直流，一部分直流电给电池充电，另一部分逆变为交流电给服务器供电。

&emsp;&emsp;同时蓄电池也可逆变给服务器供电

#### 11.2 HVDC（High Voltage Direct Current）

&emsp;&emsp;市电输入HVDC，经过整流器将交流转化为直流，并且直接使用直流给服务器供电和给蓄电池充电。

&emsp;&emsp;相较于UPS，HVDC在备份、工作原理、扩容以及蓄电池挂靠等方面存在显著的技术优势

#### 11.3服务器电源标准

- ATX标准

- SSI标准
- EPS规范：Entry Power Supply Specification
- TPS规范：Thin Power Supply Specification
- DPS规范：Distributed Power Supply Specification
- MPS规范：Midrange Power Supply Specification

#### 11.4 服务器电源指标

- 功率
- 3C认证
- 电压保持时间
- 冗余电源选择

#### 11.5服务器衡量标准，RASUM标准

- 可靠性 Reliability
- 可用性 Availability
- 可扩展性 Scalability
- 易用性 Usability
- 可管理性 Manageability

### 12、服务器发展方向

#### 12.1 纵向扩展

&emsp;&emsp;提升服务器的单台性能，主要运用于企业核心数据库和核心应用系统

#### 12.2 横向扩展

&emsp;&emsp;通过分布式架构，将工作任务拆解给多台服务器进行处理，主要运用于超大规模数据中心、大数据分析、Web应用等

#### 12.3 超融合

&emsp;&emsp;将计算、存储、网络、管理放到一个箱子中，应用于高性能数据分析、HPC、一体化数据中心

### 13、服务器应用部署架构

#### 13.1 架构分类

&emsp;&emsp;单机系统、C/S架构、B/S架构、互联网架构

#### 13.2 层次架构

![服务器层次架构图](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%B1%82%E6%AC%A1%E6%9E%B6%E6%9E%84%E5%9B%BE.png)

#### 13.3.上层软件架构

&emsp;&emsp;业务应用软件-中间件-数据库-OS和虚拟化

## 二、CPU基础知识

### 1.计算机CPU处理器经典结构

#### 1.1 冯诺依曼结构

&emsp;&emsp;程序指令集和数据存储器合并在一起，指向同一个存储器的不同位置，结构相对简单

#### 1.2 哈佛结构

&emsp;&emsp;程序指令集和数据存储器分开，通过两条总线连接，结构相对复杂，数据吞吐率相对较高

#### 1.3 冯诺依曼结构拓展

&emsp;&emsp;三个基本原则：二进制逻辑、程序存储执行、计算机由五个基本部分组成

&emsp;&emsp;计算机组成部分：运算器、控制器、存储器、输入设备和输出设备，其中运算器和控制器组合为中央处理器（CPU）

冯诺依曼结构的特点：

- 单机处理，机器以运算器为中心
- 程序存储四相
- 指令和数据均可参与运算
- 数据以二进制表示
- 软件和硬件完全分离
- 指令由操作码和操作数组成
- 指令顺序执行

![冯诺依曼结构](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E5%86%AF%E8%AF%BA%E4%BE%9D%E6%9B%BC%E7%BB%93%E6%9E%84.png)

参考文章

[【冯诺依曼结构和哈佛结构】](https://blog.csdn.net/mahoon411/article/details/119078925?ops_request_misc=&request_id=&biz_id=102&utm_term=%E5%93%88%E4%BD%9B%E7%BB%93%E6%9E%84&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-119078925.142^v68^pc_new_rank,201^v4^add_ask,213^v2^t3_esquery_v1&spm=1018.2226.3001.4187)

[【冯诺依曼结构概述】](https://blog.csdn.net/qiangwudi9847/article/details/102486255?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167210622616782427464896%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167210622616782427464896&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-102486255-null-null.142^v68^pc_new_rank,201^v4^add_ask,213^v2^t3_esquery_v1&utm_term=%E5%86%AF%C2%B7%E8%AF%BA%E4%BE%9D%E6%9B%BC%E7%BB%93%E6%9E%84&spm=1018.2226.3001.4187)

### 2.X86系统典型架构

典型芯片架构-1

- FSB：前端总线，连接CPU与北桥
- 北桥：集成内存控制器、显卡或者外接显卡总线接口
- 南桥：其他所有的外 设和总线：PCI、PCIe、USB、LPC、IDE/SATA、Audio 、GPIO、电源管理等等

典型的芯片架构-2

- CPU
  - 集成内存控制器
  - 集成PCIe总线控制器，支持的规格一般比南桥高
- ESI/DMI：CPU与南桥之间的总线
- 南桥：其他所有的外设和总线：PCI、PCIe、USB、LPC、IDE/SATA、GPIO、电源管理等等

### 3.CPU概念

&emsp;&emsp;中央处理器主要包括运算器（算术逻辑运算单元，ALU，Arithmetic Logic Unit）和高速缓冲存储器（Cache）及实现它们之间联系的数据（Data）、控制及状态的总线（Bus）

&emsp;&emsp;计算器三大核心部件：CPU、存储器、输入和输出（I/O）

### 4.缓存（Cache）介绍

&emsp;&emsp;缓存(Cache Memory)是位于CPU与内存之间的临时存储器，目前的CPU拥有一级、二级和三级缓存(L1 L2 L3 Cache)

&emsp;&emsp;共享分布式结构（ Haswell Server架构）: 共享分布式结构L3是主缓存，L3具有L2中所有数据的副本

&emsp;&emsp;私有本地结构（Skylake Server架构）: 私有缓存L2成为主缓存，共享L3缓存作为溢出缓存，L2中的数据可能不存在于L3中

### 5.缓存的作用

#### L1 Cache(一级缓存)

&emsp;&emsp;是CPU第一层高速缓存，分为数据缓存和指令缓存。内置的L1高速缓存的容量和结构对CPU的性能影响较大，L1级高速缓存的容量不可能做得太大。一般服务CPU的L1缓存的容量通常在32—256KB。

#### L2 Cache(二级缓存)

&emsp;&emsp;是CPU的第二层高速缓存，分内部和外部两种芯片。内部的芯片二级缓存运行速度与主频相同，而外部的二级缓存则只有主频的一半。L2高速缓存容量也会影响CPU的性能，原则是越大越好，服务器和工作站上用CPU的L2高速缓存更高达256-1MB，有的高达2MB或者3MB。

#### L3 Cache(三级缓存)

&emsp;&emsp;分为两种，早期的是外置，现在的都是内置的。而它的实际作用即是，L3缓存的应用可以进一步降低内存延迟，同时提升大数据量计算时理器的性能。降低内存延

迟和提升大数据量计算能力对游戏都很有帮助。而在服务器领域增加L3缓存在性能方面仍然有显著的提升。

### 6.CPU频率

主频=外频*倍频

CPU 主频为 CPU 的额定工作频率，当内核数目和缓存大小一样时，主频越高的 CPU性能越好

外频是CPU的基准频率，单位也是MHz。CPU的外频决定着整块主板的运行速度。

### 7.前端总线(Front Side Bus)频率

前端总线是CPU和外界交换数据的最主要通道，数据带宽＝(总线频率*数据位宽)/8

### 8.字长概念和扩展指令集

字长：CPU在单位时间内能一次处理的二进制数的位数叫字长

字节：8位

CPU扩展指令集：CPU用于计算和控制系统

### 9.超流水线与超标量

#### 9.1 流水线

流水线技术是一种将每条指令分解为多步，并让各步操作重叠，从而实现几条指令并行处理的技术

过程：指令预取、译码、执行、写回结果

#### 9.2 超流水线

超流水线是通过细化流水、提高主频，使得在一个机器周期内完成一个甚至多个操作，其实质是以时间换取空间

#### 9.3 超标量

超标量是通过内置多条流水线来同时执行多个处理器，其实质是以空间换取时间

参考文章：[【流水线、超流水线、超标量（superscalar）技术对比】](https://blog.csdn.net/qq_32092885/article/details/83349275?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167212753216800188596674%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=167212753216800188596674&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~pc_rank_34-3-83349275-null-null.142^v68^pc_new_rank,201^v4^add_ask,213^v2^t3_esquery_v1&utm_term=%E8%B6%85%E6%B5%81%E6%B0%B4%E7%BA%BF%E4%B8%8E%E8%B6%85%E6%A0%87%E9%87%8F&spm=1018.2226.3001.4187)

### 10.多核和超线程技术

多核：把多个CPU（核心）集成到单个集成电路芯片中

超线程：同时多线程，是一项允许一个CPU执行多个控制流的技术，需要CPU、主板、BIOS、操作系统支持

## 三、内存基础知识

### 1.DDR内存条的结构组成

内存由内存芯片、电路板、金手指等部分组成

- DDR1= 大片+圆口

- DDR2= 小片+圆口

- DDR3= 小片+方口

- DDR4= 小片+方口

### 2.DDR和SDRAM

&emsp;&emsp;DDRR内存是在SDRAM内存基础上发展而来的，仍然沿用SDRAM生产体系，一个时钟周期内传输两次次数据，它能够在时钟的上升期和下 降期各传输一次数据，因此称为双倍速率同步动态随机存储器

&emsp;&emsp;SDRAM在一个时钟周期内只传输一次数据，它是在时钟的上升期进行数据传输

### 3.DDR4内存参数

容量、额定速度、工作电压、整机支持的最大DDR4数量、整机支持的最大DDR4容量、最大工作速度

### 4.内存保护技术

#### 4.1 Parity

增加一个检查位给每个资料的字元（或字节），并且能够侦测到一个字符中所有奇（偶）同位的错误，缺点是无法定位和修复错误。

#### 4.2 ECC

一种指令纠错技术，可以发现并纠正错误，但只能检测和纠正单一错误

#### 4.3 Chipkill

是IBM公司为了解决目前服务器内存中ECC技术的不足而开发的，是一种新的ECC内存保护标准

### 5.内存条类型介绍

| 类型   | 全称                        | 主要差别                             | 特点              |
| ------ | --------------------------- | ------------------------------------ | ----------------- |
| UDIMM  | Unbuffered DIMM，无缓冲DIMM | 无Buffer芯片                         | 适用于低端CPU平台 |
| RDIMM  | Registered DIMM，           | 带寄存器的DIMM                       | 适用于主流场景    |
| LRDIMM | Load-Reduced DIMM低负载DIMM | 数据和地址控制信号都经过Register芯片 | 适用于大容量场景  |

按体积内存分为DIMM、Mini-DIMM、SODIMM（Small Outline DIMM）、MicroDIMM、VLP（Very Low Profile）、ULP（Ultra Low Profile）

### 6.NVDIMM

非易失性内存，由 JEDEC 固态技术协会所定义

- 符合 DDR4 标准
- 能够相容于标准的 DIMM 插槽
- 当系统供电中断时(有可能是突然断电或正常关机), NVDIMM会由超级电容供电, 把DRAM的数据写回NAND Storage中. 当电力恢复, 再把资料搬回 DRAM

### 7.服务器内存条配置原则

- Purley平台支持的内存类型有RDIMM和LRDIMM

- Purley平台支持的DIMM频率有：2133/2400/2666/2933(CasCade)

- 推荐采用平衡插法配置内存，所有内存通道配置一样的内存（包括速率、容量、Rank等），不支持不同类型DIMM的混插

- 多颗CPU配置时，首先保持各个CPU的内存配置一样

- 当只有一个DIMM时，必须插在给定通道的slot0槽位（离CPU最远的位置）

- 当单rank、双rank、四rank DIMM插成2DPC，总是先从最远的槽位开始插rank高的DIMM

注：2 DPC：2 DIMM per Channel (每个通道插2根DIMM条)

### 8.内存带宽计算

- 满配最大内存带宽 = 内存标称频率 × 内存总线位数 × 通道数 × CPU个数

- 实际使用的内存带宽 = 内存标称频率 × 内存总线位数 × 实际使用的通道数，举例：

- 如2路CPU，支持64根内存，通道数为6的服务器，配2666的内存条时内存的带宽为：
  2666 * 64 × 6 × 2 =2047488 Mbit/s=250GB/s

## 四、硬盘基础知识

### 1.硬盘接口分类

- 早期硬盘接口有IDE、SCSI等，随着硬盘技术发展，这些接口类型已经消失；
- 当前主流的硬盘使用的接口有SATA、SAS、FC（服务器未使用）、PCIE等
- SAS和SATA接口区分：通过红圈2区分，SATA 硬盘红圈2部分是分开的；SAS是连接在一起的，且背面有金手指。

### 2.硬盘外观尺寸

HDD按照碟片的尺寸大小，即硬盘碟片直径分为： 1 inch、1.8 inch、2.5 inch、3.5 inch、5.25inch。

SSD虽然没有碟片概念，但尺寸大小和HDD是保持一致的

- HDD主要尺寸
  - 主流的硬盘中，有3.5寸和2.5寸2种，不同硬盘厂商的外形尺寸都在公差范围内，差异极小。
  - 3.5寸：LFF，长×宽=147mm×101.85mm，企业级3.5寸硬盘的厚度一般为26.1mm 。
  - 2.5寸：SFF，长×宽= 100.45mm×69.85mm ，企业级2.5寸硬盘的厚度一般为15mm 。
- SSD主要尺寸
  - 主流的固态硬盘尺寸为2.5寸，不同硬盘厂商的外形尺寸差异微小。
  - 2.5寸：SFF，长×宽= 100.45mm×69.85mm ，SSD的高度有7mm，9.5mm和15mm，各个厂家可能设计不同的厚度，但不会超过15mm。

- 2.5寸偏重于性能表现，具有更高的IOPS和带宽；
- 3.5寸偏重于容量，容量一般比同一代的硬盘大4倍左右，在2014年，硬盘厂商已经推出的最大容量为8T，目前容量可达14TB。

### 3.硬盘关键指标

- 硬盘容量（Volume）

- 转速（Rotational speed）

- 平均访问时间（Average Access Time）

- 数据传输率（Date Transfer Rate）

- IOPS(Input/Output Per Second) 每秒读写次数，随机读写频繁的应用，此项为关键指标
- 吞吐量（Throughput）

### 4.磁盘功耗差异分析

- SSD的功耗比HDD功耗略低（除PCIe-SSD外）；
- 同类型HDD，3.5寸比2.5寸功耗高，且容量越大，功耗越大；
- HDD转速（RPM）越高，功耗越大；
- SSD的容量越大，功耗越大。

### 5.硬盘的业务类型

#### 5.1 HDD

- 企业级Performance类

- 企业级Capacity类

- 企业级云盘类

- 桌面级硬盘（Desktop Disk）类

#### 5.2 SSD

- 读密集型（Read intensive）
- 均衡型（Main Stream）
- 写密集型（Read intensive）

### 6.SSD磁盘系统构成

&emsp;&emsp;SSD主要由控制单元和存储单元（当前主要是FLASH闪存颗粒）组成，控制单元包括SSD控制器、主机接口、DRAM等，存储单元主要是NAND FLASH颗粒。

- 主机接口：主机访问SSD的协议和物理接口，常用的有SATA、SAS和PCIE等。

- SSD控制器：负责主机到后端介质的读写访问和协议转换，表项管理、数据缓存及校验等，是SSD的核心部件。

- DRAM：FTL表项和数据的缓存，以提供数据访问性能。

- NAND FLASH：数据存储的物理器件载体。

![SSD架构](https://kx-image.oss-cn-chengdu.aliyuncs.com/SSD%E6%9E%B6%E6%9E%84.png)

参考文章：[【SSD---系统架构】](https://blog.csdn.net/xinghuanmeiying/article/details/122919548?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167213401816782425698465%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167213401816782425698465&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-122919548-null-null.142^v68^pc_new_rank,201^v4^add_ask,213^v2^t3_esquery_v1&utm_term=SSD%E7%A3%81%E7%9B%98%E7%B3%BB%E7%BB%9F%E6%9E%84%E6%88%90&spm=1018.2226.3001.4187)

### 7.NVMe SSD系统结构

&emsp;&emsp;NVMe SSD的接口形态有很多种，但现在作为常见的是M.2，M.2 SSD是以M.2协议为接口的固态硬盘。M.2原名为NGFF接口，由mSATA演化而来，具有更多的尺寸选择，并且可以选择SATA和PCIe协议

### 8.NAND FLASH介绍

&emsp;&emsp;NAND FLASH是一种非易失性随机访问存储介质，基于浮栅(Floating Gate)晶体管设计，通过浮栅来锁存电荷，电荷被储存在浮栅中，在无电源供应的情况下数据仍然可以保持。

&emsp;&emsp;相对于HDD，具有读写速度快、访问时延低等特点

#### NAND FLASH内部结构

NAND内部存储单元组成包括： LUN、Plane、Block、Page、Cell。

- LUN：又称为DIE(16+1 RAID4 冗余，保护DIE、Page失效)，能够独立封装的最小物理单元，通常包含多个plane；

- Plane：拥有独立的Page寄存器，通常LUN包含1K或2K个奇数Block或偶数Block；

- Block：能够执行擦除操作的最小单元，通常由多个Page组成；

- Page：执行编程和读操作的最小单元，通常大小为4KB/8KB/16KB等；

- Cell：Page中最小操作擦写读单元，可以存储1bit或多bit。

### 9.RAID定义

RAID （Redundant Array of Independent Disks）即独立磁盘冗余阵列，RAID技术将多个单独的物理硬盘以不同的方式组合成一个逻辑硬盘，从而提高了硬盘的读写性能和数据安全性

| 名称   | 特性                                   |
| ------ | -------------------------------------- |
| RAID 0 | 数据条带化，无校验                     |
| RAID 1 | 数据镜像，无校验                       |
| RAID 3 | 数据条带化读写，校验信息存放于专用硬盘 |
| RAID 5 | 数据条带化，校验信息分布式存放         |
| RAID 6 | 数据条带化，分布式校验并提供两级冗余   |

同时采用两种不同的RAID方式还能组合成新的RAID级别

**RAID 0+1** ：先做RAID 0，后做RAID 1，同时提供数据条带化和镜像

**RAID 1+0** ：类似于RAID 0+1，区别在于先做RAID 1，后做RAID 0

**RAID 5+0** ：先做RAID 5，后做RAID 0，能有效提高RAID 5的性能

### 10.RAID数据组织及存取方式

- 分块：将一个分区分成多个大小相等的、地址相邻的块，这些块称为分块。它是组成条带的元素。
- 条带：同一磁盘阵列中的多个磁盘驱动器上的相同“位置”（或者说是相同编号）的分块。

### 11.RAID热备、重构

&emsp;&emsp;热备（HotSpare）：当冗余的RAID组中某个硬盘失效时，在不干扰当前RAID系统的正常使用的情况下，用RAID系统中另外一个正常的备用硬盘自动顶替失效硬盘，及时保证RAID系统的冗余性。热备分为全局式和专用式。

### 12.RAID卡Cache保护原理

#### 电池方案

- 系统异常下电后，DDR的数据继续保存于DDR中

- BBU电池给DDR继续供电，保证DDR的自刷新正常工作

- 数据保持时间有限，一般为48h~72h

- 工作过程中，电池需要定期充放电，影响性能约4h~9h

- 电池化学特性决定，其寿命受环境影响较大

**超级电容方案

- 系统异常下电，数据从DDR转移到Flash卡的Nand flash中
- 超级电容给控制器，DDR，Flash卡供电，保证数据转移到Flash卡上
- 数据转移完毕后即不需要超级电容供电，数据可以永久保存
- 超级电容的充放电时间非常短，对性能几乎没有影响
- 工作过程中，电容容量在持续下降，但可以保证整个生命周期的工作

### 13.RAID卡对接硬盘

- 硬盘直连：SAS/SATA硬盘直接连接到控制器上，一般应用于偏计算的高性能机架或刀片服务器上
- Expander扩展：Expander是一种在背板上的扩展芯片，控制器输出通过Expander扩展硬盘；目前支持14盘和26盘规格；一般应用在偏存储的机架服务器上

### 14.常见RAID级别的比较

| RAID级别 | RAID0 |  RAID1   |  RAID5   |  RAID6   |  RAID10  |
| :------: | :---: | :------: | :------: | :------: | :------: |
|  可靠性  | 最低  |    高    |   较高   |   最高   |    高    |
| 冗余类型 |  无   | 镜像冗余 | 校验冗余 | 校验冗余 | 镜像冗余 |
| 可用空间 | 100%  |   50%    | (N-1)/N  | (N-2)/N  |   50%    |
|   性能   | 最高  |   最低   |   较高   |   较高   |    高    |

## 五、GPU和AI加速知识

### 1.GPU基础知识

&emsp;&emsp;图形处理器（英语：Graphics Processing Unit，缩写：GPU），又称显示核心、视觉处理器、显示芯片，是一种专门在个人电脑、工作站、游戏机和一些移动设备（如平板电脑、智能手机等）上图像运算工作的微处理，是显卡或GPU卡的“心脏”。

### 2.GPU与CPU计算模式对比

#### CPU与GPU对比

- CPU是一个有多种功能的优秀领导者。它的优点在于调度、管理、协调能力强，计算能力则位于其次。而GPU相当于一个接受CPU调度的“拥有大量计算能力”的员工。

- DRAM即动态随机存取存储器，是常见的系统内存。

- Cache存储器：电脑中作高速缓冲存储器，是位于CPU和主存储器DRAM之间，规模较小，但速度很高的存储器。
- 算术逻辑单元ALU是能实现多组算术运算和逻辑运算的组合逻辑电路。

#### GPU适合深度学习的三大理由

- 高宽带的内存

- 多线程并行下的内存访问隐藏延迟

- 数量多且速度快的可调整的寄存器和L1缓存

- 更高的浮点运算能力，提供了多核并行计算的基础结构，且核心数非常多，可以支撑大量数据的并行计算

&emsp;&emsp;简而言之，CPU擅长统领全局等复杂操作，GPU擅长对大数据进行简单重复操作。CPU是从事复杂脑力劳动的教援，而GPU是进行大量并行计算的体力劳动者。

### 3.GPU通信知识

&emsp;&emsp;GPU是协处理器，与CPU端存储是分离的，故GPU运算时必须先将CPU端的代码和数据传输到GPU，GPU才能执行kernel函数。涉及CPU 与GPU通信，其中通信接口PCI-E的版本和性能会直接影响通信带宽

#### NvLink 技术

&emsp;&emsp;提供更高带宽与更多链路，并可提升多 GPU 和多 GPU/CPU 系统配置的可扩展性，因而可以解决这种互联问题。单个 NVIDIA Tesla® V100 GPU 即可支持多达六条 NVLink 链路，总带宽为 300 GB/秒，这是 PCIe 3 带宽的10 倍。 NVLink提升GPU服务器单机的GPU通信性能

#### GPUDirect RDMA技术

&emsp;&emsp;则提升了不同服务器间GPU的通信性能，其实就是计算机A的GPU可以直接访问计算机B的GPU内存 ；深度学习模型越来越复杂，计算数据量暴增，对于大规模深度学习训练任务，单机已经无法满足计算需求，多机多卡的分布式训练成为了必要的需求，这个时候多机间的通信成为了分布式训练性能的重要指标。

### 4.GPU常见计算精度

#### FP32 单精度计算

单精度的浮点数中采用4个字节也就是32位二进制来表达一个数字，1位符号，8位指数，23位小数，有效位数为7位

#### FP64 双精度计算

双精度浮点数采用8个字节也就是64位二进制来表达一个数字，1位符号，11位指数，52位小数，有效位数为16位

#### FP16 半精度计算

半精度浮点数采用2个字节也就是16位二进制来表达一个数字， 1位符号、5位指数、10位小数，有效位数为3位

### 5.不同数据精度应用领域

因为采用不同位数的浮点数的表达精度不一样，所以造成的计算误差也不一样

- 对于需要处理的数字范围大而且需要精确计算的科学计算来说，就要求采用双精度浮点数，例如：计算化学，分子建模，流体动力学

- 对于常见的多媒体和图形处理计算、深度学习、人工智能等领域，32位的单精度浮点计算已经足够了

- 对于要求精度更低的机器学习等一些应用来说，半精度16位浮点数就可以甚至8位浮点数就已经够用了

### 6.GPU虚拟化技术

- API 拦截是最古老的一种方式，工作的OpenGL 和DirectX 层。通过拦截API 中的指令，然后发送到GPU，得到结果后在返回给用户。因为所有的工作都在软件层完成，所以不需要GPU 特殊的支持。

- 虚拟化GPU 是当前桌面虚拟化的热点。通过这种方式，用户可以直接使用GPU 的一部分。与API 拦截相比较，一个优点是用户可以直接使用原厂GPU 驱动，应用程序也可以直接使用原生的功能调用，而不是一个通用的子集。

- 直通就是将GPU 直接连接到虚拟机使用。对于需要大量计算资源的任务来说，这是最好的一种方式，因为虚拟机独占了整个GPU，同时对于应用程序来说，兼容性也是最好的。

### 7.GPU关键参数和技术指标

- CUDA核心；CUDA核心数量决定了GPU并行处理的能力，在深度学习、机器学习等并行计算类业务下，CUDA核心多意味着性能好一些
- 显存容量：其主要功能就是暂时储存GPU要处理的数据和处理完毕的数据。显存容量大小决定了GPU能够加载的数据量大小。
- 显存位宽：显存在一个时钟周期内所能传送数据的位数，位数越大则瞬间所能传输的数据量越大，这是显存的重要参数之一。
- 显存频率：一定程度上反应着该显存的速度，以MHz（兆赫兹）为单位,显存频率随着显存的类型、性能的不同而不同。显存频率和位宽决定显存带宽。
- 显存带宽：指显示芯片与显存之间的数据传输速率，它以字节/秒为单位。显存带宽是决定显卡性能和速度最重要的因素之一。
- 其他指标：除了显卡通用指标外，NVIDIA还有一些针对特定场景优化的指标，例如TsnsoCore、RTCoreRT等能力。例如TensenCore专门用于加速深度学习中的张量运算

### 8.NVIDIA GPU选型策略参考

#### 具体选型方法1：依据场景，判断算法依赖的参数，从而选择具体GPU

- 如果客户用于科学计算(天文，物理，化学等学科的模拟或计算，以及数学计算)

  - 推荐参考双精度指标，推荐tesla P100（双精度性价比最高）

  - 如果客户提及模型训练（神经网络学习）

  - 推荐高显存的产品，比如P100， P40, M40等

  - GeForce系列产品内存不大于12GB

- 如果客户提及政务云（桌面云）
  - 推荐支持虚拟化的GPU产品，如：M60, M10等 （M6是非PCIe卡）

- 如果客户提及渲染，需要显卡外出线，支持HDCP 等
  - 推荐Quadro系列产品

- 若客户用于视频监控(视频分析，平安城市，行为分析等)，推荐tesla P4

#### 具体选型方法2：依据客户的推荐GPU进行选型

- 若关注性价比，推荐P4

- 若客户关注单精度最高性价比，推荐 GeForce GTX 1080Ti

### 9.NVIDIA GPU散热方式

- 显卡的散热方式分为散热片和散热片配合风扇的形式，也叫作主动式散热和被动式散热方式。

- 一般一些工作频率较低的显卡采用的都是被动式散热，这种散热方式就是在显示芯片上安装一个散热片即可，并不需要散热风扇。

- 因为较低工作频率的显卡散热量并不是很大，没有必要使用散热风扇，这样在保障显卡稳定工作的同时，不仅可以降低成本，而且还能减少使用中的噪音。

### 10.AI加速卡的主要形式

&emsp;&emsp;常见的AI加速芯片包括GPU、FPGA（Field Programmable Gate Array）和ASIC（Application SpecificIntegrated Circuit）三种类型。

&emsp;&emsp;FPGA是一种半定制芯片，灵活性强集成度高，但运算量小，量产成本高，适用于算法更新频繁或市场规模小的专用领域；

&emsp;&emsp;ASIC专用性强，市场需求量大的专用领域，但开发周期较长且难度极高。谷歌自主设计了一款基于ASIC的TPU（TensorProcessingUnit）专门用于机器学习工作负载。

## 六、服务器网卡基础知识

### 1.什么是网卡

&emsp;&emsp;网卡是计算机与局域网互连的设备，又称为网络适配器或网络接口卡NIC（Network interface Card），是构成计算机网络系统中最基本的、最重要的和必不可少的连接设备，计算机主要通过网卡接入网络。

### 2.网卡的主要功能

- 代表固定的网络地址
- 数据的发送与接收
- 数据的封装与解封

网卡包括OSI模型的物理层和数据链路层。

- 物理层定义了数据传送与接收所需要的电与光信号、线路状态、时钟基准、数据编码和电路等，并向数据链路层设备提供标准接口。

- 数据链路层则提供寻址机构、数据帧的构建、数据差错检查、传送控制、向网络层提供标准的数据接口等功能。

发送数据时，加上首部和尾部；接收数据时，剥去首部和尾部

- 链路管理：主要是CSMA/CD（冲突检测的载波监听多路访问）的实现

- 编码与译码：物理层数据的编码与译码

### 3.服务器网卡介绍

普通网卡对可靠性和安全性要求不高，而服务器一直处于运行状态，因此要求数据传输速度快、CPU占用率低、安全性能高

### 4.服务器网卡分类

- 按总线类型分类：PCIe、USB、ISA、PCI

- 按结构类型分类：集成网卡（LOM）、PCIe标卡网卡、灵活插卡、Mezz卡

- 按应用类型分类：工作站网卡、服务器专用网卡

- 按协议分类：以太网卡、FC网卡、IB网卡
- 按速率分类：100Mb、1000Mb、1Gb、10Gb、25Gb、40Gb、100Gb

- NIC：Network interface Card
  特指以太网卡，支持TCP/IP协议，应用于以太网络中
  Network Card, Gigabit , LC Fiber Optic,2 Ports , PCIE 2.0 X4-8086-1522-2
- CNA：Converged Network Adapter
  融合网卡，本质上是以太网卡，但支持FCoE功能（FC over Ethernet）
- HBA：Host Bus Adapter
  特指FC网卡，支持FC协议，连接存储或光纤交换机
- HCA：Host Channel Adapter
  ➢特指Infiniband网卡，即IB卡,应用于高带宽、低时延的高性能计算项目中；

### 5.网卡接口介绍

- 电口：PC上见到的那种网口接口,即普通的RJ45接口，连接网线

- 光口：用于连接光模块，根据接口封装形式，可以分为SFP+、SFP28、QSFP+

### 6.SmartNIC概念介绍

&emsp;&emsp;SmartNIC通过从服务器CPU卸载网络处理工作负载提高数据中心的服务器性能

&emsp;&emsp;标准网卡(NIC)和智能网卡(Smart NIC)的根本区别在于Smart NIC从主机CPU卸载的处理量。Smart NIC是围绕FPGA平台设计的，FPGA被设计为接受本地化编程,在硬件实现上，NIC和FPGA不需要做在一起，FPGA通过CCIX连入一个自带网卡的SoC

### 7.SmartNIC实现形式

SmartNIC基于DPU（数据处理单元），与传统NIC的区别为

- NIC是插入服务器或存储盒以实现到以太网网络连接的PCIe卡。
- 基于DPU的SmartNIC不仅具有简单的连接性，而且还可以在NIC上实现网络流量处理实现数据加速。
- 基于DPU的SmartNIC可以基于ASIC，FPGA和片上系统（SOC）技术实现

### 8.SmartNIC不同实现技术对比

&emsp;&emsp;ASIC具有很高的成本效益，可以提供最佳的价格性能，但其灵活性有限

&emsp;&emsp;FPGA NIC（例如Mellanox Innova-2 Flex相比之下）是高度可编程的，并且可以花费足够的时间和精力来相对有效地支持几乎任何功能（在可用门的限制内），但是FPGA难以编程且价格昂贵

&emsp;&emsp;因此，对于更复杂的用例，SOC（如Mellanox BlueField DPU可编程SmartNIC）提供了似乎是基于DPU的最佳SmartSmart实施选项：**良好的价格性能，易于编程且高度灵活**

### 9.SmartNIC网卡设计考虑

- 可编程硬件
  Programmable ASIC (Mellanox/Broadcom/QLogic)
  FPGA (Intel/Xilinx)
  SOC (Huawei/Netronome)
- 通用处理器
  ARM
  ATOM（Intel x86）
- 功率能耗：>75w超出PCIe插槽供电，需外接电源，随着处理性能提升，功耗必然增长，尤其又引入了多核处理器
- 安全隔离：SmartNIC（即NIC）是检查网络流量，阻止攻击和加密传输的第一个/最简单/最佳的位置

## 七、操作系统基础知识

### 1.操作系统基本概念

&emsp;&emsp;操作系统（operating system，缩写作 OS）是管理计算机硬件与软件资源的计算机程序，同时也是计算机系统的内核与基石。操作系统需要处理如管理与配置内存、决定系统资源供需的优先次序、控制输入与输出设备、操作网络与管理文件系统等基本事务。操作系统也提供一个让用户与系统交互的操作界面。

参考文章：[【操作系统基本概念汇总】](https://blog.csdn.net/xingchenkobe123/article/details/106605319?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167219465116782425164151%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=167219465116782425164151&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~pc_rank_34-5-106605319-null-null.142^v68^pc_new_rank,201^v4^add_ask,213^v2^t3_esquery_v1&utm_term=%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5&spm=1018.2226.3001.4187)

### 2.服务器主流的操作系统

![服务器主流操作系统](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%BB%E6%B5%81%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F.png)

windows、redhat、suse等三种OS是最常用的操作系统

### 3.RedHat简介

&emsp;&emsp;Red Hat Enterprise Linux（RHEL）是一个由Red Hat 开发的商业市场导向的 Linux 发行版，最初Red Hat Enterprise Linux 基于 Red Hat Linux，但使用较为保守的发布周期，后来版本都是基于 Fedora。大约每六个版本的 Fedora 会有一个新版本的 Red Hat Enterprise Linux 发布。

### 4.SUSE简介

&emsp;&emsp;SUSE是Linux操作系统的发行版之一，也是德国的一个发行版。SUSE属于Novell旗下的业务, SUSE Linux是开源软件，但不是免费软件。

## 八、 可信计算基础知识

### 1.可信计算概念

&emsp;&emsp;可信计算/可信用计算（Trusted Computing，TC）是一项由可信计算组（可信计算集群，前称为TCPA）推动和开发的技术。
&emsp;&emsp;可信计算包括5个关键技术概念，他们是完整可信系统所必须的，这个系统将遵从TCG（Trusted Computing Group）规范：**认证密钥、安全输入输出、内存屏蔽/受保护执行、封装存储和远程证明**。

#### 可信

&emsp;&emsp;可信是指“一个实体在实现给定目标时其行为总是如同预期一样的结果”，强调行为的结果可预测和可控制。

#### 可信计算

- 从芯片、硬件结构、BIOS和操作系统等方面综合采取措施
- 在计算机系统中建立一个信任根，信任根可信性由安全措施确保
- 再建立一条信任链，从信任根->Hardware platform->OS->App，一级度量认证一级，一级信任一级，把这种信任扩展到整个计算机系统

### 2.TPM是什么

&emsp;&emsp;TPM：即Trusted Platform Module可信平台模块的缩写，是一项安全密码处理器的国际标准，旨在使用设备中集成的专用微控制器（安全硬件）处理设备中的加密密钥

&emsp;&emsp;TPM是一个微控制器，可以存储密钥、密码和数字证书，一般嵌入在PC主板上，同样可以用在任何需要TPM功能的计算设备上，用于抵御外部软件攻击和物理偷窃，确保信息存储的安全性

TPM包含两个重要的基本组件：

- 加密引擎：执行加密，数字签名，哈希散列

- 寄存器集合(PCR)：这些寄存器存储了平台软件状态的描述

TPM功能：可信度量、可信报告、可信存储

TMP广泛运用于：笔记本电脑、台式PC、手机、服务器等地方

#### TPM必须要具备四个主要功能

- 对称：任意算法 /非对称加密：RSA算法
- 安全储存：存储私密数据
- 完整性度量：对平台进行验证，靠不靠谱 SHA-1散列算法
- 签名认证：用于完整性度量，平台对度量值进行签名，TPM对其签名进行验证

### 3.TPM硬件结构

- TPM安全芯片既是密钥生成器、又是密钥管理器件，还提供了统一的编程接口。密钥是打开加密文件的唯一钥匙。
- TPM安全芯片的一个重要作用是加强了对密钥的管理，芯片以硬件来生成、存储和管理密钥。
- TPM安全芯片可以将密钥存储在受TPM控制器保护的非易失性存储器中。

![TPM硬件结构](https://kx-image.oss-cn-chengdu.aliyuncs.com/TPM%E7%A1%AC%E4%BB%B6%E7%BB%93%E6%9E%84.png)

- 密码协处理器：实现加密，解密，签名和签名验证。
- HMAC引擎：提供数据认证码和消息认证码两部分信息来分别保证数 据和命令消息的完整性。
- 非易失性存储器：用来存储永久标识(如EK)和TPM相关的状态

参考文章：[【可信计算简介】](https://blog.csdn.net/weixin_43651292/article/details/117574276?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167218476616800192278432%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167218476616800192278432&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-1-117574276-null-null.142^v68^pc_new_rank,201^v4^add_ask,213^v2^t3_esquery_v1&utm_term=%E5%8F%AF%E4%BF%A1%E8%AE%A1%E7%AE%97&spm=1018.2226.3001.4187)

### 4.可信计算理念

&emsp;&emsp;“可信计算”是指软件和硬件能够按照它们被设计的行为运行。它的可信概念为行为可信，它建立在可信测量、可信报告、可信管理为基础的可信平台概念上。

- 建立信任链：从信任根开始到硬件平台，到操作系统，再到应用，一级度量认证一级，一级信任一级，把这种信任扩展到整个计算机系统
- 标识身份：模块内置EK身份证书，通过权威的认证平台签发数字证书，证明芯片及系统的身份
- 保护密钥：模块内置SRK存储根密钥，不可以被外部访问，从而建立一棵密钥保护树来保护各种密钥

### 5.可信启动——基本原理

1. 首先有一个可信基础模块（Trusted Building Block——TBB），其主要部分是核心启动根（Core Root of Trust for Measurement——CRTM），这是一个理论上的安全基点，是一段无法被篡改的、经过安全审核过的、可以充分信任的核心的启动代码
2. 从该理论基点开始，先度量下一级的运行代码，然后将度量的哈希结果扩展到TPM芯片的PCR寄存器中保存，为后续做远程证明的审计验证提供基础，之后再执行被度量的代码
3. 如此这样反复，上一级度量验证下一级之后再启动下一级，从而将信任链从CRTM延伸到整个系统，并可在系统启动后通过第三方的远程证明进行验证系统的可信状态

### 6.可信启动——远程证明方案

1. 需要部署远程校验服务器RA Server，本地被校验系统上部署RA Client
2. RA Server上保存有初始的度量值（此度量值一定是没有被更改的），通过RA Server和RA Client之间发送挑战响应，把RA Client上当前的度量值发送到远程RA Server上进行校验
3. 返回校验结果。

### 7.可信启动——本地证明方案

本地证明是利用TPM芯片提供的Seal/Unseal机制实现的，本方案可以检测设备启动部件的完整性，也可以检测磁盘上文件的完整性

- 密封秘密信息：在设备处于可信状态时，管理员利用Seal机制将一个或几个PCR寄存器与机密信息密封在一，保存设备当前的可信状态。
- 验证可信状态：管理员可以利用Unseal机制判断设备当前的可信状态。如果Unseal操作成功，并且Unseal出的机密信息跟密封时的机密信息一致，则说明设备处于可信状态。否则，给出具体出问题的PCR寄存器或文件名称。

### 8.TCM(Trusted Cryptography Module)介绍

&emsp;&emsp;TCM（Trusted Cryptography Module，全名可信密码模块）标准是由**国家密码管理局**联合国内一些IT企业推出的，是一种安全芯片，能有效保护PC，防止非法用户访问电脑

- TCM以密码算法为突破口，依据嵌入芯片技术，完全采用我国自主研发的密码算法和引擎，来构建一个安全芯片。
- TCM由长城、中兴、联想、同方、方正、兆日等十二家厂商联合推出，得到国家密码管理局的大力支持
- TCM安全芯片在系统平台中的作用：为系统平台和软件提供基础的安全服务，建立更为安全可靠的系统平台环境。

## 九、服务器基准测试和认证

### 1.服务器基准测试体系

- 两大基准体系：TPC、SPEC

- 四大应用中的基准测试：

  1. 高性能计算（HPC）：Linpack…

  2. 在线事务处理（OLTP）：TPC-C…

  3. Web服务：SPEC Web2005、TPC-W

  4. Java应用服务器：SPECjbb2005
     - 针对CPU性能的SPEC CPU2000、SPEC CPU2006
     - 针对Web服务器的SPEC Web2005
     - 针对高性能计算的SPEC HPC2002与SPEC MPI2006
     - 针对Java应用的SPEC jAppServer2004与SPEC JBB2005等

- 专用基准测试——Oracle基准测试，SAP基准测试等

- 关键服务器基准测试规范：TPC-C
  - TPC-C单位为tpmC，对系统在线事务处理能力进行评价，含义为每分钟内系统处理新订单的个数
- Java应用服务器：SPECjbb2005
  - 针对CPU性能的SPEC CPU2000、SPEC CPU2006
  - 针对Web服务器的SPEC Web2005
  - 针对高性能计算的SPEC HPC2002与SPEC MPI2006
  - 针对Java应用的SPEC jAppServer2004与SPEC JBB2005等

### 2.服务器认证

&emsp;&emsp;认证是指由认证机构证明产品、服务、管理体系符合相关技术规范、相关技术规范的强制性要求或者标准的合格评定活动

认证通常分为产品、管理体系和服务认证。

- 大家较为熟悉的CCC认证、安全认证就是产品认证

- 以ISO9001标准为依据开展的质量管理体系认证

- 以ISO14001标准为依据开展的环境管理体系认证

- 还有以体育场所服务标志为依据开展的体育服务认证等

认证要求包括哪些方面

1. 安全（Safety）
2. 电磁兼容（EMC）
3. 健康和环保（HE-Health/Environment）
4. 频谱协调(RF)
5. 接口性能（Telecom）
6. 认证评估程序
7. 市场监管
8. 标识标志（Labeling）

