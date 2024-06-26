---
layout: post
title: IPMI和IPMB
description: IPMI和IPMB
categories:
  - 服务器
---

# IPMI和IPMB

## 一、IPMI

IPMI全称是Intelligent Platform Management Interface，智能平台管理接口。它是由Intel、DELL、HP及NEC于1998年共同提出，同时，由IPMI论坛创建了IPMI标准依赖。

简单说IPMI就是一个面向服务器的一个管理的开源协议，该协议规定了消息格式规范，对于不同的传输接口，消息格式会有所不同。同时，除了协议规定的标准命令外，还有预留了许多oem命令，用户可以根据实际需求实现oem命令。

### 1.IPMP功能

1. 远程控制服务器开关机、重启等操作。

2. 远程查看和修改bios设置，查看系统启动过程。

3. 远程连接服务器，登入系统，解决ssh访问问题。

4. 查看系统故障日志记录访问系统事件日志 (System Event Log,SEL) 和传感器状况等。

5. 获取系统的SNMP 警报。

6. 远程安装系统，查看系统启动故障等问题。

7. 对系统进行各种设置等。

### 2.IPMP缺点

IPMI协议在实际应用中有一定的风险和一些缺点，主要是安全性和可用性：

1. 网络安全 - IPMI通信协议会留下可以通过网络攻击的漏洞。

2. 配置挑战 –在旧网络设置被扭曲时，配置IPMI任务具有挑战性。在这样的情况下，可通过系统的BIOS清除网络配置够解决问题。

3. 更新挑战 – 安装更新补丁有可能导致网络故障。例如，切换主板的端口可能会导致故障。一般重新启动系统能够解决导致网络失败的问题。

4. 数据非可视化– ipmi协议中数据是二进制，非人类可读，增加了调试和使用的难度

### 3.IPMP工作原理

IPMI协议是基于命令/响应机制，可通过网络将机箱、传感器、固件、存储、应用等信息进行分类传递，同时可通过软件对BIOS、系统管理软件、远程终端等传感器等进行分类管理，并在网络、串行/Moderm接口、IPMB（I2C）、KCS、SMIC、SMBus等不同接口上都使用统一的格式传递信息。
IPMI核心是一个使用专用芯片/控制器(一般称为服务器处理器或基板管理控制器(Baseboard Management Controller，BMC))，该控制器不依赖于服务器的处理器、BIOS或操作系统来工作，而是有常电供电并独立运行的，具有一个单独的子系统，在该系统中有ipmi的守护进程来处理主机或者远程管理的命令。一般BMC通常是一个贴片或者外挂在服务器主板上的独立的板卡，目前，部分服务器主板也提供对IPMI支持的。
在工作时，所有的IPMI命令都发给BMC，BMC收到命令后返回结果或者执行对应的操作，BMC上可以记录对应的数据或操作日志。在服务器内部，主机可以通过LPC或者其他接口发送命令给BMC来上报日志、或者其他信息。在远程管理时，可以通过命令获取到服务器的传感器状态信息、主机的调试信息（SOL）、控制服务器上下电重启等。通过远程管理可以避免在嘈杂的机房中工作。
而在命令传输的安全性方面，用户也无需担心，IPMI增强的认证(基于安全哈希算法1和基于密钥哈希消息认证)和加密(高级加密标准和Arcfour)功能有助于实现安全的远程操作。对VLAN的支持更是为设置管理专用网络提供了方便，并且可以以通道为基础进行配置

### 4.IPMP协议版本

ipmi协议只有三个版本：ipmi1.0、ipmi1.5、ipmi2.0。
IPMI1.0于1998年9月16日发布，制定了基本规范。

IPMI1.5于2001年2月21日发布，允许IPMI系统通过串口，BMC专用的带外网口，或者与主机共享的带内网口（NC-SI）与远程管理系统通讯。

IPMI2.0于2004年2月12日发布，增加了SOL（serial over LAN）、群组管理系统、身份认证（RAKP+、SHA-1等）、基于OpenSSL/RCMP+增强网络接口安全、固件防火墙和VLAN支持等。其中,SOL支持将BIOS输出和操作系统终端重定向到与BMC相连的串口，可通过IPMI与远程系统管理软件连接。同时，IPMI2.0兼容系统通常有KVM over IP(基于IP的远程键盘鼠标显示器连接)、远程桌面和页面服务器等功能，尽管这些并不属于IPMI协议。

IPMI2.0修订版1.1于2013年10月1日发布，修订了勘误表，说明和附录，并增加了对IPv6寻址。2015年4月21发布了最后一个版本，更新了GUID等。
![ipmb修订记录](https://kx-image.oss-cn-chengdu.aliyuncs.com/ipmb%E4%BF%AE%E8%AE%A2%E8%AE%B0%E5%BD%95.png)

### 5.ipmp接口

|               Interfaces Type               | Description                                                  |
| :-----------------------------------------: | :----------------------------------------------------------- |
|               IPMI Messaging                | BMC ipmi或BMC与host之间的ipmi接口，包括通道、访问权限等等    |
| Intelligent Platform Management Bus（IPMB） | 分为智能设备和非智能设备（master read/write）                |
| Intelligent Chassis Management Bus （ICMB） | ICMB桥实现访问的接口控制器，是IPMI的一种实现，包含IPMB。     |
|       Keyboard Controller Style (KCS)       | 键盘控制器 (KCS)接口，KCS接口是支持的BMC到SMS接口。KCS接口仅用于SMS消息。 |
|  Server Management Interface Chip（SMIC）   | 该接口是一个3 I / O端口接口,可以使用简单的ASIC、FPGA或离散逻辑设备实现。每个字节的握手也用于传输数据。 |
|             Block Transfer (BT)             | 提供了一个更高的性能系统接口。与KCS和SMIC接口不同,每一个块的握手都用于在接口中传输数据。 |
|        SMBus System Interface (SSIF)        | 用于主机控制器和BMC之间传输,使用SMBus读写块协议。BMC总是作为SMBus的从设备，主机控制器将数据写入到BMC。 |
|             IPMI LAN Interface              | 通过ipv4/ipv6远程连接BMC的UDP网络接口                        |
|         IPMI Serial/Modem Interface         | IPMI串口访问接口。                                           |

### 6.IPMP实现

IPMI只是一个通信协议，它的实现依赖于单独的硬件平台，即基板管理控制器（BMC）。

BMC硬件为系统和管理软件提供通信接口。BMC获取主板、模块的传感器信息和日志信息上报管理软件，同时，可接收系统或者管理软件的设置信息，以设置和管理服务器，确保服务器系统健康运行。BMC实现IPMI管理如下图所示。
常用的实现：ami、openbmc、iBMC（华为）
MCU实现（ipmb）：ipmc

## 二、IPMB

### 1.Request / Response Protocol （请求 / 应答协议）

IPMB 使用“请求——应答”协议，发送一条请求消息给一个智能设备，该设备会返回一个独立的应答消息。任何传输协议都是有限制的，IPMB 总线直接支持有 15 个内部节点的系统，系统应用应该努力减轻总线的占用时间，例如，每秒钟少于 6 条消息，这样做，可以确保节点可以成功在要求的重试次数内抢占总线。


请求消息和应答消息都是通过 I2C 总线的“主写”（Master Write）模式传输的，也就是说，一条请求消息是从一个作为 I2C 主端（Master）的节点发出，被一个作为 I2C 从端（slave）的节点接收；对应的应答消息是从一个作为 I2C 主端的应答设备（ responding intelligent device）发出，被一个作为 I2C 从端的请求发起者接收。

请求消息的一个重要性质是要能够指导应答消息能够准确返回给请求者，请求者在请求消息里提供它的 Requester‘s Slave Address （rqSA）和 Requester’s LUN （rqLUN）来引导应答返回请求者。

每个应答者的接口协议都定义了一些支持的命令字，应答者在这个特定的域位置必须提供至少一个命令字，任何其他和命令域相关的参数字节必须紧跟着第一个字节。应用程序向一个节点发送请求消息，必须能够通过解析命令域来识别应答。

有效的请求是指使用节点支持的命令的 cmd 号、netFn 号和 LUN 并且能够通过数据完整性计算的请求消息，所有有效请求必须提供相应的应答。对这一要求的例外就是当节点接收到请求时正在处理一个命令，或者在等待另一个节点的应答。这时候节点可以选择用 NAK 通知请求方，也可以[封装](https://www.eefocus.com/baike/492719.html)一个包含 C0h 完成码的应答消息来告诉请求方节点忙。

IPMB 不要求对消息进行列队处理，也就是说接口是单线的，节点一旦接到某个节点的请求会清除所有等待发送到该节点的应答消息。但是桥节点除外。如果节点接收到请求包的数据 checksum 不对，将直接丢弃该数据包。

#### 1.1 如何区分请求消息和应答消息：

请求消息和应答消息的网络功能号（network functions）不同，请求消息使用的是偶数，应答消息用的是奇数的网络功能号。由于有可能同时存在不止一条请求消息，那么区分一个应答消息到底是对应那一个请求就非常重要了。这是通过下列机制来实现的：

1、应答消息中包含 Responder’s Slave Address （rsSA） 和 Responder’s LUN （rsLUN），这回告诉请求者（Requester）应答是从哪里来的。

2、请求消息中的命令域（Command （cmd） field）也包含在应答消息里面，这可以让请求者核实应答是针对具体那一个请求的。

3、请求消息中的序列号（Seq）也会包含在应答消息中，这可以使请求者核实应答是针对那一次请求的。

#### 1.2 丢失应答消息由下面的机制来处理：

1、请求方超时等待应答。

2、请求方重试并超时等待知道超过超时次数限制。

3、请求方可以发送“Get Device ID”命令来看看应答方是否仍在工作，如果有应答，则发送“Warm Reset”命令来将其 IPMB [通信接口](https://www.eefocus.com/baike/504905.html)返回到一个可知的状态，如果没有应答，则可以认为应答方已经死掉。

IPMB 接口使用序列号域来区分从发的请求消息，但是当请求方重启，就有可能产生和上一次发送的序列号相同的请求消息来，例如序列号刚超界，或者是刚刚只发送了一条消息。为了避免这些，我们使用序列号期满机制，当接收端接收到同一个节点的序列号相同但超过了序列号期满间隔就被看做是一条新的请求消息。

#### 1.3 Response TIme-outs（应答超时）

请求方不可能认为所有的请求都会有应答，有可能在某种情况下应答方可能不会提供应答消息。这时候，请求方就要实现超时等待机制。

#### 1.4  Unexpected Request Messages（不希望的请求消息）

请求消息和应答消息不是自动成对传输的，在一个请求消息和应答消息的间隔，允许其他的总线传输，因此，节点必须容忍意外请求发生。例如，节点 A 发送请求消息给节点 B，A 正在等待应答的时候，自己可能会接到来自节点 C 的请求，节点 A 有多种方式来处理，第一，节点 A 可以接受并处理它，这是最可取的方法；如果不可以做到这一点，节点可以选择丢弃这个意外消息。

另外，任何应答消息如果没等待的请求有与之匹配的话，将被丢弃。任何希望得到的应答消息如果 corrupted 或者不遵从协议的话，也将被丢弃。

### 2.Link Layer Addressing（链路层处理）

#### 2.1 ConnecTIon Header（链路数据头）

这是一种链路层和网络层结合的数据头，成功传输该数据头就会在节点间建立连接，为传输后面的消息主体的字节准备好通路，下图为链路数据头的格式：

![链路数据头](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E9%93%BE%E8%B7%AF%E6%95%B0%E6%8D%AE%E5%A4%B4.jpg)

链路数据头包含一个 7 位的从地址，之后的一位是 I2C 读写位，由于 IPMB 协议只使用 I2C 的主写模式，所以这个读写位一直是 0。这个字节之后的是网络功能号（）和 LUN 字节，和一个 checksum 字节。如果这个 checksum 不对，节点可以拒绝接收后面的数据。

#### 2.2 Message TransacTIon Formats（消息传输格式）

IPMB 点对点传输，数据包包含 IPMB connecTIon headers、command、和 data 字节。如下图所示：

![消息传输格式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E6%B6%88%E6%81%AF%E4%BC%A0%E8%BE%93%E6%A0%BC%E5%BC%8F.jpg)

#### 2.3 除此之外，链路层还要处理一下对非智能设备的读写问题

### 3.Design Objectives（设计目标）

协议的基本设计符合下列目标和原则：

1. 设计能在处理能力有限和 RAM 和 ROM 资源有限的微处理器运行的应用。
2. 展现高度对称性，例如：对节点的访问方法要和节点的具体应用相独立。
3. 展现高度的相似性，例如机架寻址自然要和基本的 I2C 寻址一致。
4. 桥接功能的处理和存储负担最小化。
5. 展现清晰的功能层次，例如：消息数据不要差杂有协议数据，不同方面的协议数据也要相应的保持独立。

### 4.Protocol Use of I2C Services（协议对 I2C 服务的使用）

IPMB 总线协议中 I2C 是用作媒介和物理层的。协议严格按照下列条件使用 I2C 服务：

1. 只使用主写模式（Master Write）（I2C 从地址的 R/W 位一直是 0）。

2. 不使用 10_bit 寻址。

3. 每次传输只用一个从地址。

4. 不使用 repeated starts，repeated starts 是 I2C 协议为了在传输的过程中改变总线方向而定义的。由于 IPMB 协议只用到主写模式，所以就不需要 repeated starts 了，不过在非 IPMB 协议的设备中还是要继续使用 repeated starts 的。

5. I2C 的通知机制（ACK bit）只表示字节被从端接收，不能保证接收的数据的完整性和正确性。
