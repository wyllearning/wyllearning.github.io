---
layout: post
title: 设计模式（八）-- 装饰模式
description: 设计模式（八）-- 装饰模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 装饰模式

## 1.概述

动态地给一个对象增加一些额外的职责，就增加对象功能来说，装饰模式比生成子类实现更为灵活。装饰模式是一种对象结构型模式

## 2.结构图

![装饰模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E8%A3%85%E9%A5%B0%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Component（抽象构件）：它是具体构件和抽象装饰类的共同父类，声明了在具体构件中实现的业务方法，它的引入可以使客户端以一致的方式处理未被装饰的对象以及装饰之后的对象，实现客户端的透明操作。

- ConcreteComponent（具体构件）：它是抽象构件类的子类，用于定义具体的构件对象，实现了在抽象构件中声明的方法，装饰器可以给它增加额外的职责（方法）。

- Decorator（抽象装饰类）：它也是抽象构件类的子类，用于给具体构件增加职责，但是具体职责在其子类中实现。它维护一个指向抽象构件对象的引用，通过该引用可以调用装饰之前构件对象的方法，并通过其子类扩展该方法，以达到装饰的目的。

- ConcreteDecorator（具体装饰类）：它是抽象装饰类的子类，负责向构件添加新的职责。每一个具体装饰类都定义了一些新的行为，它可以调用在抽象装饰类中定义的方法，并可以增加新的方法用以扩充对象的行为。
  

## 4.C语言实现

```c
#include <stdio.h>

// 基础组件
typedef struct Component {
    void (*operation)();
} Component;

void operation() {
    printf("Component operation\n");
}

// 装饰组件A
typedef struct DecoratorA {
    Component base;
    void (*addedBehaviorA)();
} DecoratorA;

void addedBehaviorA() {
    printf("Added behavior A\n");
}

void operationA(Component* base) {
    base->operation();
    addedBehaviorA();
}

// 装饰组件B
typedef struct DecoratorB {
    Component base;
    void (*addedBehaviorB)();
} DecoratorB;

void addedBehaviorB() {
    printf("Added behavior B\n");
}

void operationB(Component* base) {
    base->operation();
    addedBehaviorB();
}

// 装饰组件C
typedef struct DecoratorC {
    Component base;
    void (*addedBehaviorC)();
} DecoratorC;

void addedBehaviorC() {
    printf("Added behavior C\n");
}

void operationC(Component* base) {
    base->operation();
    addedBehaviorC();
}

int main() {
    Component component = { &operation };
    DecoratorA decoratorA = { { &operationA }, &addedBehaviorA };
    DecoratorB decoratorB = { { &operationB }, &addedBehaviorB };
    DecoratorC decoratorC = { { &operationC }, &addedBehaviorC };

    // 基础组件
    component.operation();
    printf("\n");

    // 装饰组件A
    decoratorA.base.operation = &operationA;
    ((Component*)&decoratorA)->operation(&component);
    printf("\n");

    // 装饰组件B
    decoratorB.base.operation = &operationB;
    ((Component*)&decoratorB)->operation((Component*)&decoratorA);
    printf("\n");

    // 装饰组件C
    decoratorC.base.operation = &operationC;
    ((Component*)&decoratorC)->operation((Component*)&decoratorB);
    return 0;
}
```

运行结果:

```c
Component operation

Component operation
Added behavior A

Component operation
Added behavior A
Added behavior B

Component operation
Added behavior A
Added behavior B
Added behavior C
```

