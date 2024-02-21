---
layout: post
title: 设计模式（四）-- 适配器模式
description: 设计模式（四）-- 适配器模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
marp: true
---

# 适配器模式

## 1.概述

将一个接口转换成客户希望的另一个接口，使接口不兼容的那些类可以一起工作，其别名为包装器(Wrapper)。适配器模式既可以作为类结构型模式，也可以作为对象结构型模式

## 2.结构图

![适配器模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E9%80%82%E9%85%8D%E5%99%A8%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Target（目标抽象类）：目标抽象类定义客户所需接口，可以是一个抽象类或接口，也可以是具体类。

- Adapter（适配器类）：适配器可以调用另一个接口，作为一个转换器，对Adaptee和Target进行适配，适配器类是适配器模式的核心，在对象适配器中，它通过继承Target并关联一个Adaptee对象使二者产生联系。

- Adaptee（适配者类）：适配者即被适配的角色，它定义了一个已经存在的接口，这个接口需要适配，适配者类一般是一个具体类，包含了客户希望使用的业务方法，在某些情况下可能没有适配者类的源代码。

## 4.C语言实现

```c
#include <stdio.h>
#include <stdlib.h>

struct Adapter
{
    void (*Request)();
};

struct Adaptee
{
    void (*specificRequest)();
};

void specificRequest()
{
    printf("Specific request.\n");
}

void request(struct Adaptee *adaptee)
{
    adaptee->specificRequest();
}

int main()
{
    struct Adaptee adaptee = {specificRequest};
    struct Adapter adapter = {request};
    adapter.Request(&adaptee); 
    return 0;
}
```

该示例中，`Adaptee` 结构体是需要被适配的接口，其中包含了一个 `specificRequest` 方法。`Adapter` 结构体是适配器，它包含一个 `Adaptee` 类型的成员变量和一个 `request` 方法，该方法内部调用了 `Adaptee` 的 `specificRequest` 方法。在 main 函数中创建了一个 `Adapter` 类型的变量，并调用了它的 request 方法。
