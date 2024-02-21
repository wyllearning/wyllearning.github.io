---
layout: post
title: 设计模式（十五）-- 迭代器模式
description: 设计模式（十五）-- 迭代器模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 迭代器模式

## 1.概述

提供一种方法来访问聚合对象，而不用暴露这个对象的内部表示，其别名为游标(Cursor)。迭代器模式是一种对象行为型模式

## 2.结构图

![迭代器模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E8%BF%AD%E4%BB%A3%E5%99%A8%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Iterator（抽象迭代器）：它定义了访问和遍历元素的接口，声明了用于遍历数据元素的方法，例如：用于获取第一个元素的first()方法，用于访问下一个元素的next()方法，用于判断是否还有下一个元素的hasNext()方法，用于获取当前元素的currentItem()方法等，在具体迭代器中将实现这些方法。
- ConcreteIterator（具体迭代器）：它实现了抽象迭代器接口，完成对聚合对象的遍历，同时在具体迭代器中通过游标来记录在聚合对象中所处的当前位置，在具体实现时，游标通常是一个表示位置的非负整数。
- Aggregate（抽象聚合类）：它用于存储和管理元素对象，声明一个createIterator()方法用于创建一个迭代器对象，充当抽象迭代器工厂角色。
- ConcreteAggregate（具体聚合类）：它实现了在抽象聚合类中声明的createIterator()方法，该方法返回一个与该具体聚合类对应的具体迭代器ConcreteIterator实例。

## 4.C语言实现

```c
// 定义一个迭代器接口
struct Iterator {
    void* (*next)();
    int (*hasNext)();
};

// 定义一个聚合类
struct Aggregate {
    int* items;
    int size;
    struct Iterator* iterator;
};

// 定义一个具体迭代器类
struct ConcreteIterator {
    struct Aggregate* aggregate;
    int current;
};

// 实现迭代器接口的 next 方法
void* next(struct Iterator* iterator) {
    struct ConcreteIterator* ci = (struct ConcreteIterator*)iterator;
    if (ci->current < ci->aggregate->size) {
        return &ci->aggregate->items[ci->current++];
    }
    return NULL;
}

// 实现迭代器接口的 hasNext 方法
int hasNext(struct Iterator* iterator) {
    struct ConcreteIterator* ci = (struct ConcreteIterator*)iterator;
    return ci->current < ci->aggregate->size;
}

// 实现聚合类的迭代器方法
struct Iterator* getIterator(struct Aggregate* aggregate) {
    struct ConcreteIterator* ci = (struct ConcreteIterator*)malloc(sizeof(struct ConcreteIterator));
    ci->aggregate = aggregate;
    ci->current = 0;
    ci->iterator.next = next;
    ci->iterator.hasNext = hasNext;
    return (struct Iterator*)ci;
}

int main() {
    struct Aggregate aggregate;
    aggregate.size = 5;
    aggregate.items = (int*)malloc(sizeof(int) * aggregate.size);
    aggregate.iterator = getIterator(&aggregate);
    while (aggregate.iterator->hasNext(aggregate.iterator)) {
        int* item = (int*)aggregate.iterator->next(aggregate.iterator);
        printf("%d\n", *item);
    }
    return 0;
}
```

这是一个简单的示例，它将聚合类的元素打印到控制台
