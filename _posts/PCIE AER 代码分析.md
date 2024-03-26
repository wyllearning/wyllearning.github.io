# Flow Control FCP+分析

# Flow Control

### 名词缩写

TLP: 事务层包

FC: Flow Control 流量控制

VC: Virtual Channel

Ports: 端口

DLLP: Flow Control DLLPs

### 概述

发送端在发送前可以通过Flow Control机制知道接收端能否接收即将发送的TLP。

在PCI无FW机制，因此不能判断接收端能否接收对应的TLP，因此可能会造成等待或重发。

每个PCIe端口最多支持8个VC，并且每个VC的Flow Control Buffer是完全独立的，单个VC满后不会影响到其他VC。



Flow Control机制是通过相邻两个Ports的数据链路层之间发送DLLP来实现

在进行初始化的时候，接收端需要向发送端报告（reports）其Buffer的大小，在正常运行状态（Run-time）时，会周期性地通过Flow Control DLLPs来告知发送端，接收端的各个Buffer的大小。

Buffer和计数器（FC Counter）在事务层（Transaction Layer）中计算

## TLP

### 分类

1. Posted Transactions：Memory Writes和Messages
2. Non-Posted Transactions：Memory Reads、Configuration Reads and Writes、IO Reads and Writes
3. Completions：Read and Write Completion

### 组成

Header, Data

## Flow Control Buffer类型

Flow Control为了获得更高的数据传输效率，将这三类TLP分开存放，同时将Header与Data部分也分开存放。因此，一共存在六种不同的Flow Control Buffer类型

![blob.png](https://kx-image.oss-cn-chengdu.aliyuncs.com/1000019445-6365950769594249542315201.png)

Flow Control Buffer的存储单元（Unit）被称作Flow Control Credits。对于Header来说，Requests TLP每个unit等于5DW，而Completions TLP每个unit等于4DW。对于Data来说，每个unit等于4DW，即Data Buffer是按照16个字节对齐的

### Buffer最小值

![blob.png](https://kx-image.oss-cn-chengdu.aliyuncs.com/1000019445-6365950770569326548136521.png)

### Buffer最大值

![blob.png](https://kx-image.oss-cn-chengdu.aliyuncs.com/1000019445-6365950771599095282857663.png)

**注：**0 unit表示无限（Infinite）



## Flow Control初始化

在发送任何TPL之前，PCIe总线必须要先完成Flow Control初始化

物理层链路初始化----LinkUp----Flow Control初始化

![blob.png](https://kx-image.oss-cn-chengdu.aliyuncs.com/1000019445-6365950778362193756193756.png)

由于VC0默认初始化，Flow Control会默认初始化VC0，其他通道在使能时才会初始化

### Flow Control初始化步骤

![blob.png](https://kx-image.oss-cn-chengdu.aliyuncs.com/1000019445-6365950779646670131768547.png)

1. FC_Init1：数据链路控制 Data Link Control

PCIe设备会连续地发送三个InitFC1类型的Flow Control DLLP来报告其接收Buffer 的大小。三个DLLP的顺序是固定的：Posted、Non-Posted、Completions

![blob.png](https://kx-image.oss-cn-chengdu.aliyuncs.com/1000019445-6365950780874892075617927.png)

2. FC_Init2：管理状态机 Management State Machine

与FC_Init1类似，主要用于保证不同时间下每个设备都能初始化DLLP

## FC DLLP Buffer更新

在完成FC初始化之后，相邻的两个设备之间会周期性的通过Updated FC DLLP更新接收Buffer的大小，Update FC DLLP的格式与FC_Init的格式是类似的

![blob.png](https://kx-image.oss-cn-chengdu.aliyuncs.com/1000019445-6365950782932866999830966.png)