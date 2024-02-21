---
layout: post
title: gd32 swdp_enable 影响i2c输出
description: gd32 swdp_enable 影响i2c输出
categories:
  - GD32
tags:
  - I2C
  - GD32
---

### 一、问题简介

1. GD32处理gpio_pin_remap_config时，如果未配置为SW-DP，则9555、fru、lm75都不能正常读写
2. SPI写的时容易产生问题，出现只写一部分现象

### 二、问题分析

#### 问题1

gpio_pin_remap_config(GPIO_SWJ_SWDPENABLE_REMAP, ENABLE);

gpio_pin_remap_config：配置GPIO引脚重映射

GPIO_SWJ_SWDPENABLE_REMAP：JTAG-DP除能，SW-DP使能

可在用户手册上查表

![image-20230327114050855](https://kx-image.oss-cn-chengdu.aliyuncs.com/image-20230327114050855.png)

![](https://kx-image.oss-cn-chengdu.aliyuncs.com/image-20230327114021563.png)

JTAG-DP除能，SW-DP使能时，PB4释放为普通引脚

而我们的i2c选择引脚正好是PB4控制

如果不重定向SW-DP，则PB4引脚用于NJTRST

而9555和lm75不能读取，则是因为上电主动读取fru时主循环卡死，一直处于i2c总线忙，因此后面的命令均不可读取到

#### 问题2

SPI的NSS为软件触发，并且修改NSS脚为PA4

正好在定时器1中，hisports上报会写整个PA脚，因此会影响到SPI通信片选信号的判断

修改：在bios升级过程中，停止hisprots上报，需要测试是否会有不良后果
