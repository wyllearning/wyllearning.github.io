---
layout: post
title: 设计模式（十三）-- 命令模式
description: 设计模式（十三）-- 命令模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 命令模式

## 1.概述

将一个请求封装为一个对象，从而让我们可用不同的请求对客户进行参数化；对请求排队或者记录请求日志，以及支持可撤销的操作。命令模式是一种对象行为型模式，其别名为动作(Action)模式或事务(Transaction)模式

## 2.结构图

![命令模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E5%91%BD%E4%BB%A4%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Command（抽象命令类）：抽象命令类一般是一个抽象类或接口，在其中声明了用于执行请求的execute()等方法，通过这些方法可以调用请求接收者的相关操作。

- ConcreteCommand（具体命令类）：具体命令类是抽象命令类的子类，实现了在抽象命令类中声明的方法，它对应具体的接收者对象，将接收者对象的动作绑定其中。在实现execute()方法时，将调用接收者对象的相关操作(Action)。
- Invoker（调用者）：调用者即请求发送者，它通过命令对象来执行请求。一个调用者并不需要在设计时确定其接收者，因此它只与抽象命令类之间存在关联关系。在程序运行时可以将一个具体命令对象注入其中，再调用具体命令对象的execute()方法，从而实现间接调用请求接收者的相关操作。
- Receiver（接收者）：接收者执行与请求相关的操作，它具体实现对请求的业务处理

## 4.C语言实现

```c
#include <stdio.h>

typedef struct command command;
struct command
{
    void (*execute)(command *);
    void (*undo)(command *);
};

typedef struct Light Light;
struct Light
{
    int state;
};

typedef struct light_on_command light_on_command;
struct light_on_command
{
    command command;
    Light *light;
};

void light_on(light_on_command *l)
{
    l->light->state = 1;
    printf("Light is on\n");
}

void light_off(light_on_command *l)
{
    l->light->state = 0;
    printf("Light is off\n");
}

light_on_command light_on_command_obj = {
    .command = {.execute = (void (*)(command *))light_on,
                .undo = (void (*)(command *))light_off},
    .light = &(Light){0}};

int main()
{
    light_on_command_obj.command.execute((command *)&light_on_command_obj);
    light_on_command_obj.command.undo((command *)&light_on_command_obj);
    return 0;
}

```

在这个实现中, 我们有一个命令接口(Command)和一个具体命令(LightOnCommand)实现. LightOnCommand对象封装了对电灯(Light)对象的操作. 在main函数中, 我们可以看到, 我们可以调用execute()方法来打开电灯, 并且可以调用undo()方法来关闭电灯.
