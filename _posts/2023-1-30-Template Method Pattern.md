---
layout: post
title: 设计模式（二一）-- 模板方法模式
description: 设计模式（二一）-- 模板方法模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 模板方法模式

## 1.概述

定义一个操作中算法的框架，而将一些步骤延迟到子类中。模板方法模式使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤

## 2.结构图

![模板方法模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E6%A8%A1%E6%9D%BF%E6%96%B9%E6%B3%95%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- AbstractClass（抽象类）：在抽象类中定义了一系列基本操作(PrimitiveOperations)，这些基本操作可以是具体的，也可以是抽象的，每一个基本操作对应算法的一个步骤，在其子类中可以重定义或实现这些步骤。同时，在抽象类中实现了一个模板方法(Template Method)，用于定义一个算法的框架，模板方法不仅可以调用在抽象类中实现的基本方法，也可以调用在抽象类的子类中实现的基本方法，还可以调用其他对象中的方法。

- ConcreteClass（具体子类）：它是抽象类的子类，用于实现在父类中声明的抽象基本操作以完成子类特定算法的步骤，也可以覆盖在父类中已经实现的具体基本操作。

## 4.C语言实现

```c
#include <stdio.h>

// 抽象类
typedef struct AbstractClass AbstractClass;
struct AbstractClass
{
    void (*primitiveOperation1)(AbstractClass *);
    void (*primitiveOperation2)(AbstractClass *);
    void (*templateMethod)(AbstractClass *);
};

// 具体类
typedef struct ConcreteClass ConcreteClass;
struct ConcreteClass
{
    AbstractClass super;
};

// 具体类方法
void primitiveOperation1(AbstractClass *abstractClass)
{
    printf("具体类primitiveOperation1实现\n");
}

void primitiveOperation2(AbstractClass *abstractClass)
{
    printf("具体类primitiveOperation2实现\n");
}

void templateMethod(AbstractClass *abstractClass)
{
    printf("模板方法开始\n");
    abstractClass->primitiveOperation1(abstractClass);
    abstractClass->primitiveOperation2(abstractClass);
    printf("模板方法结束\n");
}

// 具体类初始化
void concreteClassInit(ConcreteClass *concreteClass)
{
    concreteClass->super.primitiveOperation1 = primitiveOperation1;
    concreteClass->super.primitiveOperation2 = primitiveOperation2;
    concreteClass->super.templateMethod = templateMethod;
}

int main()
{
    ConcreteClass concreteClass;
    concreteClassInit(&concreteClass);
    concreteClass.super.templateMethod(&concreteClass.super);
    return 0;
}
```

