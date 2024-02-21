---
layout: post
title: 设计模式（三）-- 原型模式
description: 设计模式（三）-- 原型模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 原型模式

## 1.概述

使用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。原型模式是一种对象创建型模式。

## 2.结构图

![原型模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E5%8E%9F%E5%9E%8B%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Prototype（抽象原型类）：它是声明克隆方法的接口，是所有具体原型类的公共父类，可以是抽象类也可以是接口，甚至还可以是具体实现类。

- ConcretePrototype（具体原型类）：它实现在抽象原型类中声明的克隆方法，在克隆方法中返回自己的一个克隆对象。

- Client（客户类）：让一个原型对象克隆自身从而创建一个新的对象，在客户类中只需要直接实例化或通过工厂方法等方式创建一个原型对象，再通过调用该对象的克隆方法即可得到多个相同的对象。由于客户类针对抽象原型类Prototype编程，因此用户可以根据需要选择具体原型类，系统具有较好的可扩展性，增加或更换具体原型类都很方便。

## 4.C语言实现

```c
struct Prototype {
    int x;
    int y;
};

Prototype* clone(Prototype* prototype) {
    Prototype* newPrototype = (Prototype*)malloc(sizeof(Prototype));
    memcpy(newPrototype, prototype, sizeof(Prototype));
    return newPrototype;
}
```

