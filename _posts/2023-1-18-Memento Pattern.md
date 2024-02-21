---
layout: post
title: 设计模式（十七）-- 备忘录模式
description: 设计模式（十七）-- 备忘录模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 备忘录模式

## 1.概述

在不破坏封装的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，这样可以在以后将对象恢复到原先保存的状态。它是一种对象行为型模式，其别名为Token

## 2.结构图

![备忘录模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E5%A4%87%E5%BF%98%E5%BD%95%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Originator（原发器）：它是一个普通类，可以创建一个备忘录，并存储它的当前内部状态，也可以使用备忘录来恢复其内部状态，一般将需要保存内部状态的类设计为原发器。

- Memento（备忘录)：存储原发器的内部状态，根据原发器来决定保存哪些内部状态。备忘录的设计一般可以参考原发器的设计，根据实际需要确定备忘录类中的属性。需要注意的是，除了原发器本身与负责人类之外，备忘录对象不能直接供其他类使用，原发器的设计在不同的编程语言中实现机制会有所不同。

- Caretaker（负责人）：负责人又称为管理者，它负责保存备忘录，但是不能对备忘录的内容进行操作或检查。在负责人类中可以存储一个或多个备忘录对象，它只负责存储对象，而不能修改对象，也无须知道对象的实现细节。
## 4.C语言实现

```c
#include <stdio.h>
#include <stdlib.h>

// 需要被记录状态的对象
typedef struct Originator Originator;
struct Originator
{
    int state;
};

// 备忘录对象
typedef struct Memento Memento;
struct Memento
{
    int state;
};

// 负责创建和恢复备忘录的对象
typedef struct CareTaker CareTaker;
struct CareTaker
{
    Memento *memento;
};

// 创建备忘录
Memento *createMemento(Originator *originator)
{
    Memento *m = (Memento *)malloc(sizeof(Memento));
    m->state = originator->state;
    return m;
}

// 恢复备忘录
void setMemento(Originator *originator, Memento *memento)
{
    originator->state = memento->state;
}

int main()
{
    Originator originator;
    originator.state = 10;

    // 创建备忘录
    CareTaker careTaker;
    careTaker.memento = createMemento(&originator);

    // 修改状态
    originator.state = 20;

    // 恢复状态
    setMemento(&originator, careTaker.memento);
    printf("Originator state: %d\n", originator.state);

    return 0;
}

```

