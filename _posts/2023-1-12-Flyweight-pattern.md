---
layout: post
title: 设计模式（十）-- 享元模式
description: 设计模式（十）-- 享元模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 享元模式

## 1.概述

运用共享技术有效地支持大量细粒度对象的复用。系统只使用少量的对象，而这些对象都很相似，状态变化很小，可以实现对象的多次复用。由于享元模式要求能够共享的对象必须是细粒度对象，因此它又称为轻量级模式，它是一种对象结构型模式

## 2.结构图

![享元模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E4%BA%AB%E5%85%83%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Flyweight（抽象享元类）：通常是一个接口或抽象类，在抽象享元类中声明了具体享元类公共的方法，这些方法可以向外界提供享元对象的内部数据（内部状态），同时也可以通过这些方法来设置外部数据（外部状态）。

- ConcreteFlyweight（具体享元类）：它实现了抽象享元类，其实例称为享元对象；在具体享元类中为内部状态提供了存储空间。通常我们可以结合单例模式来设计具体享元类，为每一个具体享元类提供唯一的享元对象。
  
- UnsharedConcreteFlyweight（非共享具体享元类）：并不是所有的抽象享元类的子类都需要被共享，不能被共享的子类可设计为非共享具体享元类；当需要一个非共享具体享元类的对象时可以直接通过实例化创建。
  
- FlyweightFactory（享元工厂类）：享元工厂类用于创建并管理享元对象，它针对抽象享元类编程，将各种类型的具体享元对象存储在一个享元池中，享元池一般设计为一个存储“键值对”的集合（也可以是其他类型的集合），可以结合工厂模式进行设计；当用户请求一个具体享元对象时，享元工厂提供一个存储在享元池中已创建的实例或者创建一个新的实例（如果不存在的话），返回新创建的实例并将其存储在享元池中。


## 4.C语言实现

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_CACHE_SIZE 100

// 享元类型
typedef struct {
    char *value;
    int refCount;
} Flyweight;

// 享元工厂类型
typedef struct {
    Flyweight flyweights[MAX_CACHE_SIZE];
    int totalFlyweights;
} FlyweightFactory;

// 初始化享元工厂
void initFlyweightFactory(FlyweightFactory *factory) {
    factory->totalFlyweights = 0;
}

// 获取一个享元
Flyweight *getFlyweight(FlyweightFactory *factory, char *value) {
    for (int i = 0; i < factory->totalFlyweights; i++) {
        if (strcmp(factory->flyweights[i].value, value) == 0) {
            factory->flyweights[i].refCount++;
            return &factory->flyweights[i];
        }
    }
    // 如果没有找到相同的享元，则创建一个新的享元
    Flyweight *newFlyweight = &factory->flyweights[factory->totalFlyweights++];
    newFlyweight->value = strdup(value);
    newFlyweight->refCount = 1;
    return newFlyweight;
}

// 释放一个享元
void releaseFlyweight(FlyweightFactory *factory, Flyweight *flyweight) {
    for (int i = 0; i < factory->totalFlyweights; i++) {
        if (&factory->flyweights[i] == flyweight) {
            if (--flyweight->refCount == 0) {
                free(flyweight->value);
                memmove(&factory->flyweights[i], &factory->flyweights[i+1], sizeof(Flyweight) * (factory->totalFlyweights - i - 1));
                factory->totalFlyweights--;
            }
            break;
        }
    }
}

int main() {
    FlyweightFactory factory;
    initFlyweightFactory(&factory);
    Flyweight *f1 = getFlyweight(&factory, "hello");
    Flyweight *f2 = getFlyweight(&factory, "world");
    Flyweight *f3 = getFlyweight(&factory, "hello");
    printf("f1: %p, value: %s, refCount: %d\n", f1, f1->value, f1->refCount);
    printf("f2: %p, value: %s, refCount: %d\n", f2, f2->value, f2->refCount);
    printf("f3: %p, value: %s, refCount: %d\n", f3, f3->value, f3->refCount);
    releaseFlyweight(&factory, f1);
    releaseFlyweight(&factory, f2);
    releaseFlyweight(&factory, f3);
    return 0;
}
```

我们使用了 FlyweightFactory 类型来管理享元对象。我们使用 getFlyweight() 函数来获取一个享元对象，并使用 releaseFlyweight() 函数来释放一个享元对象。当我们获取相同的享元时，它会引用计数增加，并返回同一个对象的指针，而不是创建新的对象
