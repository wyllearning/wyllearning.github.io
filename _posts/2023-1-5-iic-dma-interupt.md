---
layout: post
title: IIC DMA发送和接收并采用中断获取信息
description: IIC DMA发送和接收并采用中断获取信息
categories:
  - GD32
tags:
  - GD32
  - I2C
  - DMA
---

# IIC DMA发送和接收并采用中断获取信息

## 一、使用背景

目前项目中采用的I2C发送和接收均采用软件发送和接收，为了优化效率，更新I2C驱动为DMA发送和接收，并提供相应的接口，采用I2C接收停止中断来通知数据处理程序进行处理，可采用标志位、信号量、任务通知的方式进行。

## 二、初始化

### 1.I2C初始化

此部分初始化与普通I2C初始化相同，初始化完成后I2C默认处于从机模式，产生I2C起始信号后变为主机模式。

```c
#define I2C0_GPIO_PROT    RCU_GPIOB     // I2C0 映射的IO口号
#define I2C0_SCL_GPIO_PIN GPIO_PIN_6    // I2C0 SCL 映射的IO口
#define I2C0_SDA_GPIO_PIN GPIO_PIN_7    // I2C0 SDA 映射的IO口

#define I2C1_GPIO_PROT    RCU_GPIOB      // I2C1 映射的IO口号
#define I2C1_SCL_GPIO_PIN GPIO_PIN_10    // I2C1 SCL 映射的IO口
#define I2C1_SDA_GPIO_PIN GPIO_PIN_11    // I2C1 SDA 映射的IO口

/***************************************************
 * 名称: i2c_master_init
 * 描述: 主机初始化
 * 参数: void
 * 返回: void
 ***************************************************/
void i2c_master_init(void)
{
    // 主机初始化
    rcu_periph_clock_enable(RCU_GPIOB);    // 使能GPIOB时钟
    rcu_periph_clock_enable(RCU_I2C0);     // 使能I2C0时钟

    gpio_af_set(GPIOB, GPIO_AF_4, GPIO_PIN_6);    // 配置I2C0_SCL到PB6
    gpio_af_set(GPIOB, GPIO_AF_4, GPIO_PIN_7);    // 配置I2C0_SDA到PB7

    // 配置I2C所用的IO口
    gpio_mode_set(GPIOB, GPIO_MODE_AF, GPIO_PUPD_PULLUP, GPIO_PIN_6);
    gpio_output_options_set(GPIOB, GPIO_OTYPE_OD, GPIO_OSPEED_50MHZ, GPIO_PIN_6);
    gpio_mode_set(GPIOB, GPIO_MODE_AF, GPIO_PUPD_PULLUP, GPIO_PIN_7);
    gpio_output_options_set(GPIOB, GPIO_OTYPE_OD, GPIO_OSPEED_50MHZ, GPIO_PIN_7);

    i2c_clock_config(I2C0, 100000, I2C_DTCY_2);    // 配置I2C时钟
    i2c_mode_addr_config(I2C0, I2C_I2CMODE_ENABLE, I2C_ADDFORMAT_7BITS,
                         I2C0_OWN_ADDRESS);    // 配置I2C地址
    i2c_enable(I2C0);                          // 使能I2C0
    i2c_ack_config(I2C0, I2C_ACK_ENABLE);
}

/***************************************************
 * 名称: i2c_slave_init
 * 描述: 从机初始化
 * 参数: void
 * 返回: void
 ***************************************************/
void i2c_slave_init(void)
{
    rcu_periph_clock_enable(RCU_GPIOB);    // 使能GPIOB时钟
    rcu_periph_clock_enable(RCU_I2C1);     // 使能I2C0时钟

    gpio_af_set(GPIOB, GPIO_AF_4, GPIO_PIN_10);    // 配置I2C1_SCL到PB10
    gpio_af_set(GPIOB, GPIO_AF_4, GPIO_PIN_11);    // 配置I2C1_SDA到PB11

    // 配置I2C所用的IO口
    gpio_mode_set(GPIOB, GPIO_MODE_AF, GPIO_PUPD_PULLUP, GPIO_PIN_10);
    gpio_output_options_set(GPIOB, GPIO_OTYPE_OD, GPIO_OSPEED_50MHZ, GPIO_PIN_10);
    gpio_mode_set(GPIOB, GPIO_MODE_AF, GPIO_PUPD_PULLUP, GPIO_PIN_11);
    gpio_output_options_set(GPIOB, GPIO_OTYPE_OD, GPIO_OSPEED_50MHZ, GPIO_PIN_11);

    i2c_clock_config(I2C1, 100000, I2C_DTCY_2);                                               // 配置I2C时钟
    i2c_mode_addr_config(I2C1, I2C_I2CMODE_ENABLE, I2C_ADDFORMAT_7BITS, I2C0_OWN_ADDRESS);    // 配置I2C地址
    i2c_enable(I2C1);                                                                         // 使能I2C1
    i2c_ack_config(I2C1, I2C_ACK_ENABLE);
}
```

其实此处的主机和从机并没有实际意义，优化后重新封装I2C初始化函数

```c
/***************************************************
 * 名称: i2c_init
 * 描述: i2c初始化函数封装
 * 参数: port：I2C端口号
 *      speed：I2C速度
 *      addr：主从机地址
 * 返回: void
 ***************************************************/
void i2c_init(uint32_t port, uint32_t speed, uint8_t addr)
{
    switch (port) {
        case I2C0: {
            rcu_periph_clock_enable(I2C0_GPIO_PROT);    // 使能GPIO时钟
            rcu_periph_clock_enable(RCU_I2C0);          // 使能I2C0时钟

            gpio_af_set(I2C0_GPIO_PROT, GPIO_AF_4, I2C0_SCL_GPIO_PIN);    // 配置I2C0_SCL
            gpio_af_set(I2C0_GPIO_PROT, GPIO_AF_4, I2C0_SDA_GPIO_PIN);    // 配置I2C0_SDA

            // 配置I2C所用的IO口
            gpio_mode_set(I2C0_GPIO_PROT, GPIO_MODE_AF, GPIO_PUPD_PULLUP, I2C0_SCL_GPIO_PIN);
            gpio_output_options_set(I2C0_GPIO_PROT, GPIO_OTYPE_OD, GPIO_OSPEED_50MHZ, I2C0_SCL_GPIO_PIN);
            gpio_mode_set(I2C0_GPIO_PROT, GPIO_MODE_AF, GPIO_PUPD_PULLUP, I2C0_SDA_GPIO_PIN);
            gpio_output_options_set(I2C0_GPIO_PROT, GPIO_OTYPE_OD, GPIO_OSPEED_50MHZ, I2C0_SDA_GPIO_PIN);

            i2c_clock_config(I2C0, speed, I2C_DTCY_2);                                    // 配置I2C时钟
            i2c_mode_addr_config(I2C0, I2C_I2CMODE_ENABLE, I2C_ADDFORMAT_7BITS, addr);    // 配置I2C地址
            i2c_enable(I2C0);                                                             // 使能I2C0
            i2c_ack_config(I2C0, I2C_ACK_ENABLE);

            break;
        }
        case I2C1: {
            rcu_periph_clock_enable(I2C1_GPIO_PROT);    // 使能GPIO时钟
            rcu_periph_clock_enable(RCU_I2C1);          // 使能I2C1时钟

            gpio_af_set(I2C1_GPIO_PROT, GPIO_AF_4, I2C1_SCL_GPIO_PIN);    // 配置I2C1_SCL
            gpio_af_set(I2C1_GPIO_PROT, GPIO_AF_4, I2C1_SDA_GPIO_PIN);    // 配置I2C1_SDA

            // 配置I2C所用的IO口
            gpio_mode_set(I2C1_GPIO_PROT, GPIO_MODE_AF, GPIO_PUPD_PULLUP, I2C1_SCL_GPIO_PIN);
            gpio_output_options_set(I2C1_GPIO_PROT, GPIO_OTYPE_OD, GPIO_OSPEED_50MHZ, I2C1_SCL_GPIO_PIN);
            gpio_mode_set(I2C1_GPIO_PROT, GPIO_MODE_AF, GPIO_PUPD_PULLUP, I2C1_SDA_GPIO_PIN);
            gpio_output_options_set(I2C1_GPIO_PROT, GPIO_OTYPE_OD, GPIO_OSPEED_50MHZ, I2C1_SDA_GPIO_PIN);

            i2c_clock_config(I2C1, speed, I2C_DTCY_2);                                    // 配置I2C时钟
            i2c_mode_addr_config(I2C1, I2C_I2CMODE_ENABLE, I2C_ADDFORMAT_7BITS, addr);    // 配置I2C地址
            i2c_enable(I2C1);                                                             // 使能I2C0
            i2c_ack_config(I2C1, I2C_ACK_ENABLE);

            break;
        }
        default: break;
    }
}
```

### 2.中断初始化

此处我们使用中断主要用于获取是否I2C接收完毕，因此只需要初始化I2C接收事件中断

```c
/***************************************************
 * 名称: i2c_nvic_init
 * 描述: i2c中断初始化
 * 参数: void
 * 返回: void
 ***************************************************/
void i2c_nvic_init(void)
{
    // 中断优先级分组
    nvic_priority_group_set(NVIC_PRIGROUP_PRE1_SUB3);
    // 配置中断优先级
    nvic_irq_enable(I2C1_EV_IRQn, 0, 2);
    // 使能I2C1接收中断
    i2c_interrupt_enable(I2C1, I2C_INT_EV);
}
```

### 3.DMA初始化

DMA用于I2C的发送与接收，因此需要初始化DMA发送和接收的通道

```c
#define I2C0_TX_PERIPH  DMA0            // I2C0 TX DMA0
#define I2C0_TX_CHANNEL DMA_CH6         // I2C0 TX CH6
#define I2C0_RX_PERIPH  DMA0            // I2C0 RX DMA0
#define I2C0_RX_CHANNEL DMA_CH5         // I2C0 RX CH5
#define I2C0_TX_SUBPERI DMA_SUBPERI1    // I2C0 TX 通道
#define I2C0_RX_SUBPERI DMA_SUBPERI1    // I2C0 RX 通道

#define I2C1_TX_PERIPH  DMA0            // I2C1 TX DMA0
#define I2C1_TX_CHANNEL DMA_CH7         // I2C1 TX CH7
#define I2C1_RX_PERIPH  DMA0            // I2C1 RX DMA0
#define I2C1_RX_CHANNEL DMA_CH2         // I2C1 RX CH2
#define I2C1_TX_SUBPERI DMA_SUBPERI7    // I2C1 TX 通道
#define I2C1_RX_SUBPERI DMA_SUBPERI7    // I2C1 RX 通道

#define I2C0_DATA_ADDRESS 0x40005410    // I2C0寄存器地址 0x40005410 I2C_DATA(I2C0)
#define I2C1_DATA_ADDRESS 0x40005810    // I2C1寄存器地址 0x40005810 I2C_DATA(I2C1)


/***************************************************
 * 名称: i2c_dma_init
 * 描述: i2c dma初始化
 * 参数: void
 * 返回: void
 ***************************************************/
void i2c_dma_init(uint32_t port)
{
    dma_single_data_parameter_struct dma_init_struct;    // 初始化DMA配置结构体
    rcu_periph_clock_enable(RCU_DMA0);                   // 使能DMA0时钟
    switch (port) {
        case I2C0: {
            // DMA发送配置
            dma_deinit(I2C0_TX_PERIPH, I2C0_TX_CHANNEL);                                     // 复位DMA寄存器
            dma_init_struct.direction = DMA_MEMORY_TO_PERIPH;                                // DMA方向配置
            dma_init_struct.memory0_addr = (uint32_t)i2c0_tx_buff;                           // 内存地址
            dma_init_struct.memory_inc = DMA_MEMORY_INCREASE_ENABLE;                         // 内存自增配置
            dma_init_struct.periph_memory_width = DMA_PERIPH_WIDTH_8BIT;                     // 数据宽度
            dma_init_struct.number = I2C0_TX_BUFF_LEN;                                       // DMA数据长度
            dma_init_struct.periph_addr = I2C0_DATA_ADDRESS;                                 // 外设地址配置
            dma_init_struct.periph_inc = DMA_PERIPH_INCREASE_DISABLE;                        // 外设地址自增配置
            dma_init_struct.priority = DMA_PRIORITY_ULTRA_HIGH;                              // DMA优先级
            dma_single_data_mode_init(I2C0_TX_PERIPH, I2C0_TX_CHANNEL, &dma_init_struct);    // 初始化

            dma_circulation_disable(I2C0_TX_PERIPH, I2C0_TX_CHANNEL);                              // 循环模式关闭
            dma_channel_subperipheral_select(I2C0_TX_PERIPH, I2C0_TX_CHANNEL, I2C0_TX_SUBPERI);    // 指定DMA外围设备
            // DMA接收配置
            dma_deinit(I2C0_RX_PERIPH, I2C0_RX_CHANNEL);                                     // 复位DMA寄存器
            dma_init_struct.direction = DMA_PERIPH_TO_MEMORY;                                // DMA方向配置
            dma_init_struct.memory0_addr = (uint32_t)i2c0_rx_buff;                           // 内存地址
            dma_init_struct.memory_inc = DMA_MEMORY_INCREASE_ENABLE;                         // 内存自增配置
            dma_init_struct.periph_memory_width = DMA_PERIPH_WIDTH_8BIT;                     // 数据宽度
            dma_init_struct.number = I2C0_RX_BUFF_LEN;                                       // DMA数据长度
            dma_init_struct.periph_addr = I2C0_DATA_ADDRESS;                                 // 外设地址配置
            dma_init_struct.periph_inc = DMA_PERIPH_INCREASE_DISABLE;                        // 外设地址自增配置
            dma_init_struct.priority = DMA_PRIORITY_HIGH;                                    // DMA优先级
            dma_single_data_mode_init(I2C0_RX_PERIPH, I2C0_RX_CHANNEL, &dma_init_struct);    // 初始化

            dma_circulation_disable(I2C0_RX_PERIPH, I2C0_RX_CHANNEL);                              // 循环模式关闭
            dma_channel_subperipheral_select(I2C0_RX_PERIPH, I2C0_RX_CHANNEL, I2C0_RX_SUBPERI);    // 指定DMA外围设备
            break;
        }
        case I2C1: {
            // DMA发送配置
            dma_deinit(I2C1_TX_PERIPH, I2C1_TX_CHANNEL);                    // 复位DMA寄存器
            dma_init_struct.direction = DMA_MEMORY_TO_PERIPH;               // DMA方向配置
            dma_init_struct.memory0_addr = (uint32_t)i2c1_tx_buff;          // 内存地址
            dma_init_struct.memory_inc = DMA_MEMORY_INCREASE_ENABLE;        // 内存自增配置
            dma_init_struct.periph_memory_width = DMA_PERIPH_WIDTH_8BIT;    // 数据宽度
            dma_init_struct.number = I2C1_TX_BUFF_LEN;                      // DMA数据长度
            dma_init_struct.periph_addr = I2C1_DATA_ADDRESS;                // 外设地址配置
            dma_init_struct.periph_inc = DMA_PERIPH_INCREASE_DISABLE;       // 外设地址自增配置
            dma_init_struct.priority = DMA_PRIORITY_ULTRA_HIGH;             // DMA优先级
            dma_single_data_mode_init(I2C1_TX_PERIPH, I2C1_TX_CHANNEL,
                                      &dma_init_struct);    // 初始化

            dma_circulation_disable(I2C1_TX_PERIPH,
                                    I2C1_TX_CHANNEL);    // 循环模式关闭
            dma_channel_subperipheral_select(I2C1_TX_PERIPH, I2C1_TX_CHANNEL,
                                             I2C1_TX_SUBPERI);    // 指定DMA外围设备
            // DMA接收配置
            dma_deinit(I2C1_RX_PERIPH, I2C1_RX_CHANNEL);                    // 复位DMA寄存器
            dma_init_struct.direction = DMA_PERIPH_TO_MEMORY;               // DMA方向配置
            dma_init_struct.memory0_addr = (uint32_t)i2c1_rx_buff;          // 内存地址
            dma_init_struct.memory_inc = DMA_MEMORY_INCREASE_ENABLE;        // 内存自增配置
            dma_init_struct.periph_memory_width = DMA_PERIPH_WIDTH_8BIT;    // 数据宽度
            dma_init_struct.number = I2C1_RX_BUFF_LEN;                      // DMA数据长度
            dma_init_struct.periph_addr = I2C1_DATA_ADDRESS;                // 外设地址配置
            dma_init_struct.periph_inc = DMA_PERIPH_INCREASE_DISABLE;       // 外设地址自增配置
            dma_init_struct.priority = DMA_PRIORITY_HIGH;                   // DMA优先级
            dma_single_data_mode_init(I2C1_RX_PERIPH, I2C1_RX_CHANNEL,
                                      &dma_init_struct);    // 初始化

            dma_circulation_disable(I2C1_RX_PERIPH,
                                    I2C1_RX_CHANNEL);    // 循环模式关闭
            dma_channel_subperipheral_select(I2C1_RX_PERIPH, I2C1_RX_CHANNEL,
                                             I2C1_RX_SUBPERI);    // 指定DMA外围设备
            break;
        }
        default: break;
    }
}
```

## 二、中断函数

### 1.采用标志位做任务通知

```c
/***************************************************
 * 名称: I2C1_EV_IRQHandler
 * 描述: I2C1 事件中断
 * 参数: void
 * 返回: void
 ***************************************************/
void I2C1_EV_IRQHandler(void)
{
    if (i2c_interrupt_flag_get(I2C1, I2C_INT_FLAG_ADDSEND)) {
        i2c_interrupt_flag_clear(I2C1, I2C_INT_FLAG_ADDSEND);    // 清除ADDSEND位
    }
    else if (i2c_interrupt_flag_get(I2C1, I2C_INT_FLAG_RBNE)) {
    }
    else if (i2c_interrupt_flag_get(I2C1, I2C_INT_FLAG_STPDET)) {
        i2c_interrupt_flag_clear(I2C1, I2C_INT_FLAG_STPDET);    // 清STPDET位
        i2c_enable(I2C1);
        i2c_recv_finish_flag = true;    // 发送接收完成任务通知
    }
}
```

当其他任务中的`i2c_recv_finish_flag`为`true`时，开始处理接收到的I2C数据。

### 2. 采用信号量做任务通知

```c
i2c_recv_finish_binary_semaphore = xSemaphoreCreateBinary(); // 创建二值信号量

/***************************************************
 * 名称: I2C1_EV_IRQHandler
 * 描述: I2C1 事件中断
 * 参数: void
 * 返回: void
 ***************************************************/
void I2C1_EV_IRQHandler(void)
{
    if (i2c_interrupt_flag_get(I2C1, I2C_INT_FLAG_ADDSEND)) {
        i2c_interrupt_flag_clear(I2C1, I2C_INT_FLAG_ADDSEND);    // 清除ADDSEND位
    }
    else if (i2c_interrupt_flag_get(I2C1, I2C_INT_FLAG_RBNE)) {
    }
    else if (i2c_interrupt_flag_get(I2C1, I2C_INT_FLAG_STPDET)) {
        i2c_interrupt_flag_clear(I2C1, I2C_INT_FLAG_STPDET);    // 清STPDET位
        i2c_enable(I2C1);
        xSemaphoreGive(i2c_recv_finish_binary_semaphore);    // 发送接收完成任务通知
    }
}
```

### 3.采用任务通知

```c
/***************************************************
 * 名称: I2C1_EV_IRQHandler
 * 描述: I2C1 事件中断
 * 参数: void
 * 返回: void
 ***************************************************/
void I2C1_EV_IRQHandler(void)
{
    if (i2c_interrupt_flag_get(I2C1, I2C_INT_FLAG_ADDSEND)) {
        i2c_interrupt_flag_clear(I2C1, I2C_INT_FLAG_ADDSEND);    // 清除ADDSEND位
    }
    else if (i2c_interrupt_flag_get(I2C1, I2C_INT_FLAG_RBNE)) {
    }
    else if (i2c_interrupt_flag_get(I2C1, I2C_INT_FLAG_STPDET)) {
        i2c_interrupt_flag_clear(I2C1, I2C_INT_FLAG_STPDET);    // 清STPDET位
        i2c_enable(I2C1);
        xTaskNotifyGive((TaskHandle_t)i2c_data_manage_task_handler);    // 发送接收完成任务通知
    }
}
```

### 4.几种通知方式的区别

标志位做判断需要一个任务一直轮询标志位状态，比较浪费时间，并且如果有多个任务访问或更改这个标志位，可能会产生异常。

任务通知相对于信号量存在速度更快，占用内存更小的优点，但是任务通知只能通知到单个任务，信号量则能够完成更多更复杂的通知。

## 三、DMA发送测试函数

```c
/***************************************************
 * 名称: i2c_dma_send
 * 描述: I2C DMA发送数据
 * 参数: void
 * 返回: void
 ***************************************************/
void i2c_dma_send(void)
{
    while (i2c_flag_get(I2C0, I2C_FLAG_I2CBSY)) {};                    // 等待I2C总线空闲
    i2c_start_on_bus(I2C0);                                            // 发送起始标志位
    while (!i2c_flag_get(I2C0, I2C_FLAG_SBSEND)) {};                   // 等待SBSEND
    i2c_master_addressing(I2C0, I2C0_OWN_ADDRESS, I2C_TRANSMITTER);    // 发送从机地址
    while (!i2c_flag_get(I2C0, I2C_FLAG_ADDSEND)) {};                  // 等待ADDSEND
    i2c_flag_clear(I2C0, I2C_FLAG_ADDSEND);                            // 清ADDSEND

    i2c_dma_enable(I2C1, I2C_DMA_ON);    // DMA使能
    i2c_dma_enable(I2C0, I2C_DMA_ON);    // DMA使能

    dma_channel_enable(I2C0_TX_PERIPH, I2C0_TX_CHANNEL);    // DMA通道使能
    dma_channel_enable(I2C1_RX_PERIPH, I2C1_RX_CHANNEL);    // DMA通道使能

    while (!dma_flag_get(I2C0_TX_PERIPH, I2C0_TX_CHANNEL, DMA_FLAG_FTF)) {};    // 等待DMA全满
    while (!dma_flag_get(I2C1_RX_PERIPH, I2C1_RX_CHANNEL, DMA_FLAG_FTF)) {};    // 等待DMA全满

    i2c_stop_on_bus(I2C0);    // 发送停止信号

    while (I2C_CTL0(I2C0) & 0x0200) {};    // 等待停止信号接收
}
```

DMA接收通道应保持一直使能，DMA发送通道在需要发送时使能

## 四、DMA通道查询

目前I2C0和I2C1均已通过用户手册查得各个参数的初始化值，即宏定义中表示的参数：

```c
#define I2C0_TX_PERIPH  DMA0            // I2C0 TX DMA0
#define I2C0_TX_CHANNEL DMA_CH6         // I2C0 TX CH6
#define I2C0_RX_PERIPH  DMA0            // I2C0 RX DMA0
#define I2C0_RX_CHANNEL DMA_CH5         // I2C0 RX CH5
#define I2C0_TX_SUBPERI DMA_SUBPERI1    // I2C0 TX 通道
#define I2C0_RX_SUBPERI DMA_SUBPERI1    // I2C0 RX 通道

#define I2C1_TX_PERIPH  DMA0            // I2C1 TX DMA0
#define I2C1_TX_CHANNEL DMA_CH7         // I2C1 TX CH7
#define I2C1_RX_PERIPH  DMA0            // I2C1 RX DMA0
#define I2C1_RX_CHANNEL DMA_CH2         // I2C1 RX CH2
#define I2C1_TX_SUBPERI DMA_SUBPERI7    // I2C1 TX 通道
#define I2C1_RX_SUBPERI DMA_SUBPERI7    // I2C1 RX 通道

#define I2C0_DATA_ADDRESS 0x40005410    // I2C0寄存器地址 0x40005410 I2C_DATA(I2C0)
#define I2C1_DATA_ADDRESS 0x40005810    // I2C1寄存器地址 0x40005810 I2C_DATA(I2C1)
```

例如GD32FXX系列的DMA通道可通过下表查询

<img src="https://kx-image.oss-cn-chengdu.aliyuncs.com/DMA0%E9%80%9A%E9%81%93%E6%9F%A5%E8%AF%A2.png" alt="DMA0通道查询"  />
