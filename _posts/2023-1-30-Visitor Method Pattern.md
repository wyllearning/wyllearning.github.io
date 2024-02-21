---
layout: post
title: 设计模式（二二）-- 访问者模式
description: 设计模式（二二）-- 访问者模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 访问者模式

## 1.概述

提供一个作用于某对象结构中的各元素的操作表示，它使我们可以在不改变各元素的类的前提下定义作用于这些元素的新操作。访问者模式是一种对象行为型模式

## 2.结构图

![访问者模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E8%AE%BF%E9%97%AE%E8%80%85%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Vistor（抽象访问者）：抽象访问者为对象结构中每一个具体元素类ConcreteElement声明一个访问操作，从这个操作的名称或参数类型可以清楚知道需要访问的具体元素的类型，具体访问者需要实现这些操作方法，定义对这些元素的访问操作。


- ConcreteVisitor（具体访问者）：具体访问者实现了每个由抽象访问者声明的操作，每一个操作用于访问对象结构中一种类型的元素。

- Element（抽象元素）：抽象元素一般是抽象类或者接口，它定义一个accept()方法，该方法通常以一个抽象访问者作为参数。

- ConcreteElement（具体元素）：具体元素实现了accept()方法，在accept()方法中调用访问者的访问方法以便完成对一个元素的操作。

- ObjectStructure（对象结构）：对象结构是一个元素的集合，它用于存放元素对象，并且提供了遍历其内部元素的方法。它可以结合组合模式来实现，也可以是一个简单的集合对象，如一个List对象或一个Set对象。

## 4.C语言实现

```c
#include <stdio.h>

// 声明抽象元素类型
typedef struct Element Element;
struct Element {
  void (*Accept)(Element *self, void *visitor);
};

// 声明抽象访问者类型
typedef struct Visitor Visitor;
struct Visitor {
  void (*VisitA)(Visitor *self, Element *elementA);
  void (*VisitB)(Visitor *self, Element *elementB);
};

// 元素A
typedef struct ElementA ElementA;
struct ElementA {
  Element base;
  int valueA;
};

void ElementA_Accept(Element *self, void *visitor) {
  Visitor *v = visitor;
  v->VisitA(v, (ElementA *) self);
}

// 元素B
typedef struct ElementB ElementB;
struct ElementB {
  Element base;
  int valueB;
};

void ElementB_Accept(Element *self, void *visitor) {
  Visitor *v = visitor;
  v->VisitB(v, (ElementB *) self);
}

// 访问者
typedef struct ConcreteVisitor ConcreteVisitor;
struct ConcreteVisitor {
  Visitor base;
};

void ConcreteVisitor_VisitA(Visitor *self, ElementA *elementA) {
  printf("Visiting ElementA with value %d\n", elementA->valueA);
}

void ConcreteVisitor_VisitB(Visitor *self, ElementB *elementB) {
  printf("Visiting ElementB with value %d\n", elementB->valueB);
}

int main() {
  ElementA a = {
    { ElementA_Accept },
    10
  };
  
  ElementB b = {
    { ElementB_Accept },
    20
  };

  ConcreteVisitor v = {
    { ConcreteVisitor_VisitA, ConcreteVisitor_VisitB }
  };

  a.base.Accept((Element *) &a, (Visitor *) &v);
  b.base.Accept((Element *) &b, (Visitor *) &v);

  return 0;
}

```

