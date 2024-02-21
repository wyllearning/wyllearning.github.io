---
layout: post
title: GD32 I2C主机发送和从机中断接收
description: GD32 I2C主机发送和从机中断接收
categories:
  - GD32
tags:
  - GD32
  - I2C
---

# GD32 I2C主机发送和从机中断接收

## 一、I2C初始化

此程序采用I2C0作为主机，I2C1作为从机，因此需要初始化I2C0和I2C1

```c
/***************************************************
 * 名称: i2c_init
 * 描述: i2c初始化函数
 * 参数: void
 * 返回: void
 ***************************************************/
void i2c_init(void)
{
    i2c_master_init();    // 主机初始化
    i2c_slave_init();     // 从机初始化
    i2c_nvic_init();      // i2c中断初始化
}

```

### 1.主机初始化

```c
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

    i2c_clock_config(I2C0, 100000, I2C_DTCY_2);                                               // 配置I2C时钟
    i2c_mode_addr_config(I2C0, I2C_I2CMODE_ENABLE, I2C_ADDFORMAT_7BITS, I2C0_OWN_ADDRESS);    // 配置I2C地址
    i2c_enable(I2C0);                                                                         // 使能I2C0
    i2c_ack_config(I2C0, I2C_ACK_ENABLE);
}
```

### 2.从机初始化

```c
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

### 3.中断配置

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
    nvic_irq_enable(I2C0_EV_IRQn, 0, 3);
    nvic_irq_enable(I2C1_EV_IRQn, 0, 4);
    nvic_irq_enable(I2C0_ER_IRQn, 0, 2);
    nvic_irq_enable(I2C1_ER_IRQn, 0, 1);

    // 使能I2C0中断
    i2c_interrupt_enable(I2C0, I2C_INT_ERR);
    i2c_interrupt_enable(I2C0, I2C_INT_EV);
    i2c_interrupt_enable(I2C0, I2C_INT_BUF);

    // 使能I2C1中断
    i2c_interrupt_enable(I2C1, I2C_INT_ERR);
    i2c_interrupt_enable(I2C1, I2C_INT_EV);
    i2c_interrupt_enable(I2C1, I2C_INT_BUF);
}
```



## 二、中断服务函数

### 1.I2C0事件中断

由于I2C0作为主机的发送端，因此需要

1. 在检测到起始位中断后发送从地址
2. 检测到从机回复了地址检测后清除相关标志位
3. 检测到发送数据寄存器为空后发送数据

```c
/***************************************************
 * 名称: I2C0_EV_IRQHandler
 * 描述: I2C0 事件中断
 * 参数: void
 * 返回: void
 ***************************************************/
void I2C0_EV_IRQHandler(void)
{
    if (i2c_interrupt_flag_get(I2C0, I2C_INT_FLAG_SBSEND)) {
        i2c_master_addressing(I2C0, I2C1_SLAVE_ADDRESS, I2C_TRANSMITTER);    // 发送从机地址
    }
    else if (i2c_interrupt_flag_get(I2C0, I2C_INT_FLAG_ADDSEND)) {
        i2c_interrupt_flag_clear(I2C0, I2C_INT_FLAG_ADDSEND);    // 清ADDSEND标志位
    }
    else if (i2c_interrupt_flag_get(I2C0, I2C_INT_FLAG_TBE)) {
        if (I2C_nBytes > 0) {
            i2c_data_transmit(I2C0, *i2c_txbuffer++);    // 发送数据
            I2C_nBytes--;
        }
        else {
            i2c_stop_on_bus(I2C0);    // 发送停止位
            i2c_interrupt_disable(I2C0, I2C_INT_ERR);
            i2c_interrupt_disable(I2C0, I2C_INT_BUF);
            i2c_interrupt_disable(I2C0, I2C_INT_EV);
        }
    }
}
```

### 2.I2C1事件中断

I2C1作为从机，负责接收主机I2C0发送的数据，因此需要

1. 检测到主机发送了地址后进行回复
2. 检测到数据寄存器为非空时记录数据
3. 检测到主机STOP信号后给出接收完毕的信号

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
        *i2c_rxbuffer++ = i2c_data_receive(I2C1);
        // recv_buf[recv_buf_len++] = *(i2c_rxbuffer - 1);    // 记录接收数据
    }
    else if (i2c_interrupt_flag_get(I2C1, I2C_INT_FLAG_STPDET)) {
		i2c_recv_finish_flag = true;
        i2c_enable(I2C1);
        i2c_interrupt_disable(I2C1, I2C_INT_ERR);
        i2c_interrupt_disable(I2C1, I2C_INT_BUF);
        i2c_interrupt_disable(I2C1, I2C_INT_EV);
    }
}
```

## 三、测试函数

```c
/***************************************************
 * 名称: i2c_test
 * 描述: i2c接收测试
 * 参数: void
 * 返回: void
 ***************************************************/
void i2c_test(void)
{
    uint8_t i = 0;
    for (i = 0; i < 16; i++) {
        i2c_transmitter[i] = test_num++; // 测试数据赋值
    }
    // 由于i2c_txbuffer和i2c_rxbuffer申请时为空指针，因此需要在这里指向地址
    i2c_txbuffer = i2c_transmitter;
    i2c_rxbuffer = i2c_buffer_receiver;
    I2C_nBytes = 16;
    // 中断使能
    i2c_interrupt_enable(I2C0, I2C_INT_ERR);
    i2c_interrupt_enable(I2C0, I2C_INT_EV);
    i2c_interrupt_enable(I2C0, I2C_INT_BUF);
    i2c_interrupt_enable(I2C1, I2C_INT_ERR);
    i2c_interrupt_enable(I2C1, I2C_INT_EV);
    i2c_interrupt_enable(I2C1, I2C_INT_BUF);
    // 主机等待空闲
    while (i2c_flag_get(I2C0, I2C_FLAG_I2CBSY)) {};
    // 主机发送起始位
    i2c_start_on_bus(I2C0);

    while (I2C_nBytes > 0) {};
}
```

## 四、测试结果

### 1.连线

![连线图](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E8%BF%9E%E7%BA%BF%E5%9B%BE.jpg)

### 2.数据变化



