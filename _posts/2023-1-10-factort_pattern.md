---
layout: post
title: 设计模式（一）-- 工厂模式
description: 设计模式（一）-- 工厂模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 工厂模式

## 一、简单工厂模式

### 1.概述

定义一个工厂类，工厂类只负责创建对象，不负责实际调用相应的函数

创建对象时，根据输入参数的不同，创建不同的对象。简单来讲，你需要什么对象，输入一个正确的参数，就可以获得你想要的对象，无需知道它的创建细节

### 2.结构图

![简单工厂模式结构图](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E7%AE%80%E5%8D%95%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8F%E7%BB%93%E6%9E%84%E5%9B%BE.png)

### 3.角色

- Factory（工厂角色）：工厂类，负责实现创建所有产品实例的内部逻辑，厂类可以被外界直接调用，在工厂类中提供了静态的工厂方法`factoryMethod()`，它的返回类型为抽象产品类型Product

- Product（抽象产品角色）：它是工厂类所创建的所有对象的父类，封装了各种产品对象的公有方法，它的引入将提高系统的灵活性，使得在工厂类中只需定义一个通用的工厂方法，因为所有创建的具体产品对象都是其子类对象

- ConcreteProduct（具体产品角色）：它是简单工厂模式的创建目标，所有被创建的对象都充当这个角色的某个具体类的实例。每一个具体产品角色都继承了抽象产品角色，需要实现在抽象产品中声明的抽象方法

### 4.C语言实现

```c
#include <stdio.h>

// 定义产品接口
typedef struct {
    const char* type;
    void (*print)();
} Product;

// 具体产品1
typedef struct {
    Product product;
    int value;
} ConcreteProductA;

// 具体产品2
typedef struct {
    Product product;
    int value;
} ConcreteProductB;

// 实现产品函数
void printA() {
    printf("I am Product A\n");
}

void printB() {
    printf("I am Product B\n");
}

// 简单工厂函数
Product* createProduct(const char* type, int value) {
    if (strcmp(type, "A") == 0) {
        ConcreteProductA* productA = (ConcreteProductA*)malloc(sizeof(ConcreteProductA));
        productA->product.type = "A";
        productA->product.print = printA;
        productA->value = value;
        return (Product*)productA;
    } else if (strcmp(type, "B") == 0) {
        ConcreteProductB* productB = (ConcreteProductB*)malloc(sizeof(ConcreteProductB));
        productB->product.type = "B";
        productB->product.print = printB;
        productB->value = value;
        return (Product*)productB;
    }
    return NULL;
}

int main() {
    Product* productA = createProduct("A", 5);
    productA->print(); // 输出: "I am Product A"
    Product* productB = createProduct("B", 10);
    productB->print(); // 输出: "I am Product B"
    return 0;
}
```

## 二、工厂方法模式

### 1.概述

不再提供一个统一的工厂类来创建所有的产品对象，而是针对不同的产品提供不同的工厂，系统提供一个与产品等级结构对应的工厂等级结构

### 2.结构图

![工厂方法模式结构图](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E5%B7%A5%E5%8E%82%E6%96%B9%E6%B3%95%E6%A8%A1%E5%BC%8F%E7%BB%93%E6%9E%84%E5%9B%BE.png)

![日志记录器结构图](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E6%97%A5%E5%BF%97%E8%AE%B0%E5%BD%95%E5%99%A8%E7%BB%93%E6%9E%84%E5%9B%BE.png)

### 3.角色

- product（抽象产品）：产品对象的公共父类
- concrete_product（具体产品）：它实现了抽象产品接口
- factory（抽象工厂）：声明了工厂方法，是工厂方法模式的核心，所有创建对象的工厂类都必须实现该接口
- concrete_factory（具体工厂）：它是抽象工厂类的子类，实现了抽象工厂中定义的工厂方法，并可由客户端调用，返回一个具体产品类的实例。

### 4.C语言实现

```c
#include <stdio.h>

// 基类
typedef struct {
    const char* name;
    void (*print)(const char*);
} Shape;

// 派生类
typedef struct {
    Shape shape;
    int radius;
} Circle;

// 实现基类函数
void print(const char* name) {
    printf("I am a shape named %s\n", name);
}

// 派生类工厂函数
Shape* create_circle(int radius) {
    Circle* circle = (Circle*)malloc(sizeof(Circle));
    circle->shape.name = "Circle";
    circle->shape.print = print;
    circle->radius = radius;
    return (Shape*)circle;
}

int main() {
    //使用工厂方法创建Circle对象
    Shape* circle = create_circle(5);
    circle->print(circle->name); // 输出: "I am a shape named Circle"
    return 0;
}
```

## 三、抽象工厂模式

### 1.概述

抽象工厂模式为创建一组对象提供了一种解决方案。与工厂方法模式相比，抽象工厂模式中的具体工厂不只是创建一种产品，它负责创建一族产品。它提供一个创建一系列相关或相互依赖对象的接口，而无须指定它们具体的类。抽象工厂模式又称为Kit模式，它是一种对象创建型模式。

### 2.结构图

![抽象工厂模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E6%8A%BD%E8%B1%A1%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8F.png)

### 3.角色

- AbstractFactory（抽象工厂）：它声明了一组用于创建一族产品的方法，每一个方法对应一种产品。

- ConcreteFactory（具体工厂）：它实现了在抽象工厂中声明的创建产品的方法，生成一组具体产品，这些产品构成了一个产品族，每一个产品都位于某个产品等级结构中。

- AbstractProduct（抽象产品）：它为每种产品声明接口，在抽象产品中声明了产品所具有的业务方法。

- ConcreteProduct（具体产品）：它定义具体工厂生产的具体产品对象，实现抽象产品接口中声明的业务方法。

### 4.C语言实现

```c
#include <stdio.h>

// 定义抽象产品
typedef struct {
    const char* name;
    void (*print)();
} Product;

// 具体产品
typedef struct {
    Product product;
    int value;
} ConcreteProductA;

typedef struct {
    Product product;
    int value;
} ConcreteProductB;

// 抽象工厂
typedef struct {
    Product* (*createProductA)(int);
    Product* (*createProductB)(int);
} Factory;

// 实现产品函数
void printA() {
    printf("I am Product A\n");
}

void printB() {
    printf("I am Product B\n");
}

// 具体工厂
Product* createProductA(int value) {
    ConcreteProductA* productA = (ConcreteProductA*)malloc(sizeof(ConcreteProductA));
    productA->product.name = "Product A";
    productA->product.print = printA;
    productA->value = value;
    return (Product*)productA;
}

Product* createProductB(int value) {
    ConcreteProductB* productB = (ConcreteProductB*)malloc(sizeof(ConcreteProductB));
    productB->product.name = "Product B";
    productB->product.print = printB;
    productB->value = value;
    return (Product*)productB;
}

Factory factory = {createProductA, createProductB};

int main() {
    Product* productA = factory.createProductA(5);
    productA->print(); // 输出: "I am Product A"
    Product* productB = factory.createProductB(10);
    productB->print(); // 输出: "I am Product B"
    return 0;
}
```

