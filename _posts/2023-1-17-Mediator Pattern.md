---
layout: post
title: 设计模式（十六）-- 中介者模式
description: 设计模式（十六）-- 中介者模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 中介者模式

## 1.概述

用一个中介对象（中介者）来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。中介者模式又称为调停者模式，它是一种对象行为型模式

## 2.结构图

![中介者模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E4%B8%AD%E4%BB%8B%E8%80%85%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Mediator（抽象中介者）：它定义一个接口，该接口用于与各同事对象之间进行通信。

- ConcreteMediator（具体中介者）：它是抽象中介者的子类，通过协调各个同事对象来实现协作行为，它维持了对各个同事对象的引用。

- Colleague（抽象同事类）：它定义各个同事类公有的方法，并声明了一些抽象方法来供子类实现，同时它维持了一个对抽象中介者类的引用，其子类可以通过该引用来与中介者通信。

- ConcreteColleague（具体同事类）：它是抽象同事类的子类；每一个同事对象在需要和其他同事对象通信时，先与中介者通信，通过中介者来间接完成与其他同事类的通信；在具体同事类中实现了在抽象同事类中声明的抽象方法

## 4.C语言实现

```c
#include <stdio.h>

// 抽象组件
typedef struct Component Component;
struct Component
{
    void (*operation)(Component *);
};

// 具体组件
typedef struct ConcreteComponent ConcreteComponent;
struct ConcreteComponent
{
    Component component;
};

// 中介者
typedef struct Mediator Mediator;
struct Mediator
{
    void (*mediate)(Mediator *, Component *, Component *);
};

// 具体中介者
typedef struct ConcreteMediator ConcreteMediator;
struct ConcreteMediator
{
    Mediator mediator;
    ConcreteComponent *component1;
    ConcreteComponent *component2;
};

// 组件操作
void componentOperation(Component *component)
{
    printf("Component operation\n");
}

// 中介者操作
void mediate(Mediator *mediator, Component *component1, Component *component2)
{
    ConcreteMediator *m = (ConcreteMediator *)mediator;
    m->component1->component.operation(&(m->component1->component));
    m->component2->component.operation(&(m->component2->component));
}

int main()
{
    ConcreteComponent component1;
    component1.component.operation = componentOperation;

    ConcreteComponent component2;
    component2.component.operation = componentOperation;

    ConcreteMediator mediator;
    mediator.mediator.mediate = mediate;
    mediator.component1 = &component1;
    mediator.component2 = &component2;

    mediator.mediator.mediate(&mediator, &component1.component,
                              &component2.component);
    return 0;
}

```

