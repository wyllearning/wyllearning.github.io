---
layout: post
title: 设计模式（六）-- 桥接模式
description: 设计模式（六）-- 桥接模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 桥接模式

## 1.概述

将抽象部分与它的实现部分分离，使它们都可以独立地变化。它是一种对象结构型模式，又称为柄体(Handle and Body)模式或接口(Interface)模式

## 2.结构图

![桥接模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E6%A1%A5%E6%8E%A5%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Abstraction（抽象类）：用于定义抽象类的接口，它一般是抽象类而不是接口，其中定义了一个Implementor（实现类接口）类型的对象并可以维护该对象，它与Implementor之间具有关联关系，它既可以包含抽象业务方法，也可以包含具体业务方法。

- RefinedAbstraction（扩充抽象类）：扩充由Abstraction定义的接口，通常情况下它不再是抽象类而是具体类，它实现了在Abstraction中声明的抽象业务方法，在RefinedAbstraction中可以调用在Implementor中定义的业务方法。

- Implementor（实现类接口）：定义实现类的接口，这个接口不一定要与Abstraction的接口完全一致，事实上这两个接口可以完全不同，一般而言，Implementor接口仅提供基本操作，而Abstraction定义的接口可能会做更多更复杂的操作。Implementor接口对这些基本操作进行了声明，而具体实现交给其子类。通过关联关系，在Abstraction中不仅拥有自己的方法，还可以调用到Implementor中定义的方法，使用关联关系来替代继承关系。

- ConcreteImplementor（具体实现类）：具体实现Implementor接口，在不同的ConcreteImplementor中提供基本操作的不同实现，在程序运行时，ConcreteImplementor对象将替换其父类对象，提供给抽象类具体的业务操作方法。

## 4.C语言实现

```c
#include <stdio.h>

// 类
typedef struct TV TV;
struct TV{
  void (*turnOn)(TV*);
  void (*turnOff)(TV*);
  void (*setChannel)(TV*, int);
  void (*setVolume)(TV*, int);
};

typedef struct {
    TV tv;
    int channel;
    int volume;
} SonyTV;

typedef struct {
    TV tv;
    int channel;
    int volume;
} SamsungTV;

// 方法
void SonyTV_turnOn(TV* this){
    printf("Sony TV is turned on\n");
}

void SonyTV_turnOff(TV* this){
    printf("Sony TV is turned off\n");
}

void SonyTV_setChannel(TV* this, int channel){
    SonyTV* self = (SonyTV*) this;
    self->channel = channel;
    printf("Sony TV channel set to %d\n", channel);
}

void SonyTV_setVolume(TV* this, int volume){
    SonyTV* self = (SonyTV*) this;
    self->volume = volume;
    printf("Sony TV volume set to %d\n", volume);
}

void SamsungTV_turnOn(TV* this){
    printf("Samsung TV is turned on\n");
}

void SamsungTV_turnOff(TV* this){
    printf("Samsung TV is turned off\n");
}

void SamsungTV_setChannel(TV* this, int channel){
    SamsungTV* self = (SamsungTV*) this;
    self->channel = channel;
    printf("Samsung TV channel set to %d\n", channel);
}

void SamsungTV_setVolume(TV* this, int volume){
    SamsungTV* self = (SamsungTV*) this;
    self->volume = volume;
    printf("Samsung TV volume set to %d\n", volume);
}

// 调用
int main(void) {
    SonyTV SonyTV1;
    SonyTV1.tv.turnOn = SonyTV_turnOn;
    SonyTV1.tv.turnOff = SonyTV_turnOff;
    SonyTV1.tv.setChannel = SonyTV_setChannel;
    SonyTV1.tv.setVolume = SonyTV_setVolume;

    SamsungTV SamsungTV1;
    SamsungTV1.tv.turnOn = SamsungTV_turnOn;
    SamsungTV1.tv.turnOff = SamsungTV_turnOff;
    SamsungTV1.tv.setChannel = SamsungTV_setChannel;
    SamsungTV1.tv.setVolume = SamsungTV_setVolume;

    SonyTV1.tv.turnOn(&SonyTV1.tv);
    SonyTV1.tv.setChannel(&SonyTV1.tv, 5);
    SonyTV1.tv.setVolume(&SonyTV1.tv, 20);
    SonyTV1.tv.turnOff(&SonyTV1.tv);

    SamsungTV1.tv.turnOn(&SamsungTV1.tv);
    SamsungTV1.tv.setChannel(&SamsungTV1.tv, 10);
    SamsungTV1.tv.setVolume(&SamsungTV1.tv, 30);
    SamsungTV1.tv.turnOff(&SamsungTV1.tv);
    return 0;
}
```

在这个例子中，我们定义了两个具体实现类 SonyTV 和 SamsungTV，它们都有自己的 turnOn, turnOff, setChannel, setVolume 方法，这些方法都是直接调用具体实现类中定义的函数。这样，就可以使用抽象类作为接口，而不必关心实际使用的具体实现类。这样做可以使得抽象部分和实现部分的变化是相对独立的，并且使程序更具灵活性。
