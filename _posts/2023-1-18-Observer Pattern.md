---
layout: post
title: 设计模式（十八）-- 观察者模式
description: 设计模式（十八）-- 观察者模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 观察者模式

## 1.概述

定义对象之间的一种一对多依赖关系，使得每当一个对象状态发生改变时，其相关依赖对象皆得到通知并被自动更新。观察者模式的别名包括发布-订阅（Publish/Subscribe）模式、模型-视图（Model/View）模式、源-监听器（Source/Listener）模式或从属者（Dependents）模式。观察者模式是一种对象行为型模式

## 2.结构图

![观察者模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Subject（目标）：目标又称为主题，它是指被观察的对象。在目标中定义了一个观察者集合，一个观察目标可以接受任意数量的观察者来观察，它提供一系列方法来增加和删除观察者对象，同时它定义了通知方法notify()。目标类可以是接口，也可以是抽象类或具体类。
  
- ConcreteSubject（具体目标）：具体目标是目标类的子类，通常它包含有经常发生改变的数据，当它的状态发生改变时，向它的各个观察者发出通知；同时它还实现了在目标类中定义的抽象业务逻辑方法（如果有的话）。如果无须扩展目标类，则具体目标类可以省略。
  
- Observer（观察者）：观察者将对观察目标的改变做出反应，观察者一般定义为接口，该接口声明了更新数据的方法update()，因此又称为抽象观察者。
  
- ConcreteObserver（具体观察者）：在具体观察者中维护一个指向具体目标对象的引用，它存储具体观察者的有关状态，这些状态需要和具体目标的状态保持一致；它实现了在抽象观察者Observer中定义的update()方法。通常在实现时，可以调用具体目标类的attach()方法将自己添加到目标类的集合中或通过detach()方法将自己从目标类的集合中删除。
## 4.C语言实现

```c
#include <stdio.h>
#include <stdlib.h>

// 观察者接口
typedef void (*update_func)(int);

// 主题
typedef struct
{
    int state;
    update_func *observers;
    int observer_count;
} Subject;

// 添加观察者
void subject_add_observer(Subject *s, update_func observer)
{
    s->observers =
        realloc(s->observers, sizeof(update_func) * (s->observer_count + 1));
    s->observers[s->observer_count++] = observer;
}

// 移除观察者
void subject_remove_observer(Subject *s, update_func observer)
{
    for (int i = 0; i < s->observer_count; i++)
    {
        if (s->observers[i] == observer)
        {
            s->observers[i] = s->observers[--s->observer_count];
            break;
        }
    }
}

// 通知所有观察者
void subject_notify(Subject *s)
{
    for (int i = 0; i < s->observer_count; i++)
    {
        s->observers[i](s->state);
    }
}

// 观察者1
void observer1(int state)
{
    printf("Observer 1: State changed to %d\n", state);
}

// 观察者2
void observer2(int state)
{
    printf("Observer 2: State changed to %d\n", state);
}

int main()
{
    Subject subject = {0};
    subject_add_observer(&subject, observer1);
    subject_add_observer(&subject, observer2);
    subject.state = 1;
    subject_notify(&subject);
    subject_remove_observer(&subject, observer1);
    subject.state = 2;
    subject_notify(&subject);
    return 0;
}

```

