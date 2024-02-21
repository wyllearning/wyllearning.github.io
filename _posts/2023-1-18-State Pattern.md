---
layout: post
title: 设计模式（十九）-- 状态模式
description: 设计模式（十九）-- 状态模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 状态模式

## 1.概述

允许一个对象在其内部状态改变时改变它的行为，对象看起来似乎修改了它的类。其别名为状态对象(Objects for States)，状态模式是一种对象行为型模式

## 2.结构图

![状态模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E7%8A%B6%E6%80%81%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Context（环境类）：环境类又称为上下文类，它是拥有多种状态的对象。由于环境类的状态存在多样性且在不同状态下对象的行为有所不同，因此将状态独立出去形成单独的状态类。在环境类中维护一个抽象状态类State的实例，这个实例定义当前状态，在具体实现时，它是一个State子类的对象。
  
- State（抽象状态类）：它用于定义一个接口以封装与环境类的一个特定状态相关的行为，在抽象状态类中声明了各种不同状态对应的方法，而在其子类中实现类这些方法，由于不同状态下对象的行为可能不同，因此在不同子类中方法的实现可能存在不同，相同的方法可以写在抽象状态类中。
  
- ConcreteState（具体状态类）：它是抽象状态类的子类，每一个子类实现一个与环境类的一个状态相关的行为，每一个具体状态类对应环境的一个具体状态，不同的具体状态类其行为有所不同。
  
## 4.C语言实现

```c
#include <stdio.h>

enum State
{
    STATE_A,
    STATE_B,
    NUM_STATES
};

struct Object
{
    const struct StateFunctions *state;
    int x;
};

struct StateFunctions
{
    void (*const func1)(struct Object *);
    void (*const func2)(struct Object *);
};

static void stateAFunc1(struct Object *obj)
{
    printf("State A, func1, x = %d\n", obj->x);
}

static void stateAFunc2(struct Object *obj)
{
    printf("State A, func2, x = %d\n", obj->x);
}

static void stateBFunc1(struct Object *obj)
{
    printf("State B, func1, x = %d\n", obj->x);
}

static void stateBFunc2(struct Object *obj)
{
    printf("State B, func2, x = %d\n", obj->x);
}

static const struct StateFunctions stateFunctions[NUM_STATES] = {
    {stateAFunc1, stateAFunc2},
    {stateBFunc1, stateBFunc2},
};

static void changeState(struct Object *obj, enum State newState)
{
    obj->state = &stateFunctions[newState];
}

int main()
{
    struct Object obj = {&stateFunctions[STATE_A], 0};
    obj.state->func1(&obj);
    obj.state->func2(&obj);
    changeState(&obj, STATE_B);
    obj.state->func1(&obj);
    obj.state->func2(&obj);

    return 0;
}
```

