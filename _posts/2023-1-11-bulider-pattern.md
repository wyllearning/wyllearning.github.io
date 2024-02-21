---
layout: post
title: 设计模式（七）-- 建造者模式
description: 设计模式（七）-- 建造者模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 建造者模式

## 1.概述

建造者模式一步一步创建一个复杂的对象，它允许用户只通过指定复杂对象的类型和内容就可以构建它们，用户不需要知道内部的具体构建细节

## 2.结构图

![建造者模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E5%BB%BA%E9%80%A0%E8%80%85%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Builder（抽象建造者）：它为创建一个产品Product对象的各个部件指定抽象接口，在该接口中一般声明两类方法，一类方法是buildPartX()，它们用于创建复杂对象的各个部件；另一类方法是getResult()，它们用于返回复杂对象。Builder既可以是抽象类，也可以是接口。

- ConcreteBuilder（具体建造者）：它实现了Builder接口，实现各个部件的具体构造和装配方法，定义并明确它所创建的复杂对象，也可以提供一个方法返回创建好的复杂产品对象。

- Product（产品角色）：它是被构建的复杂对象，包含多个组成部件，具体建造者创建该产品的内部表示并定义它的装配过程。

-  Director（指挥者）：指挥者又称为导演类，它负责安排复杂对象的建造次序，指挥者与抽象建造者之间存在关联关系，可以在其construct()建造方法中调用建造者对象的部件构造与装配方法，完成复杂对象的建造。客户端一般只需要与指挥者进行交互，在客户端确定具体建造者的类型，并实例化具体建造者对象（也可以通过配置文件和反射机制），然后通过指挥者类的构造函数或者Setter方法将该对象传入指挥者类中。

## 4.C语言实现

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct _product {
    int data1;
    int data2;
    int data3;
} Product;

typedef struct _builder {
    Product* product;
    void (*buildData1)(struct _builder*, int);
    void (*buildData2)(struct _builder*, int);
    void (*buildData3)(struct _builder*, int);
} Builder;

void buildData1(Builder* builder, int data) {
    builder->product->data1 = data;
}

void buildData2(Builder* builder, int data) {
    builder->product->data2 = data;
}

void buildData3(Builder* builder, int data) {
    builder->product->data3 = data;
}

Builder* createBuilder() {
    Builder* builder = (Builder*)malloc(sizeof(Builder));
    builder->product = (Product*)malloc(sizeof(Product));
    builder->buildData1 = buildData1;
    builder->buildData2 = buildData2;
    builder->buildData3 = buildData3;
    return builder;
}

int main() {
    Builder* builder = createBuilder();
    builder->buildData1(builder, 1);
    builder->buildData2(builder, 2);
    builder->buildData3(builder, 3);

    Product* product = builder->product;
    printf("Product: data1=%d, data2=%d, data3=%d\n", product->data1, product->data2, product->data3);

    free(builder->product);
    free(builder);
    return 0;
}
```

在上面的代码中，`Product` 结构体表示要构建的最终产品，其中包含了三个整型数据。`Builder` 结构体表示建造者，其中包含了三个方法，用于构建产品中的三个数据。在 `main` 函数中，通过调用这三个方法来构建产品，最后打印出产品中的数据。
