---
layout: post
title: 设计模式（十二）-- 职责链模式
description: 设计模式（十二）-- 职责链模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 职责链模式

## 1.概述

避免请求发送者与接收者耦合在一起，让多个对象都有可能接收请求，将这些对象连接成一条链，并且沿着这条链传递请求，直到有对象处理它为止。职责链模式是一种对象行为型模式

## 2.结构图

![职责链模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E8%81%8C%E8%B4%A3%E9%93%BE%E6%A8%A1%E5%BC%8F.png)

## 3.角色

-  Handler（抽象处理者）：它定义了一个处理请求的接口，一般设计为抽象类，由于不同的具体处理者处理请求的方式不同，因此在其中定义了抽象请求处理方法。因为每一个处理者的下家还是一个处理者，因此在抽象处理者中定义了一个抽象处理者类型的对象（如结构图中的successor），作为其对下家的引用。通过该引用，处理者可以连成一条链。

-  ConcreteHandler（具体处理者）：它是抽象处理者的子类，可以处理用户请求，在具体处理者类中实现了抽象处理者中定义的抽象请求处理方法，在处理请求之前需要进行判断，看是否有相应的处理权限，如果可以处理请求就处理它，否则将请求转发给后继者；在具体处理者中可以访问链中下一个对象，以便请求的转发


## 4.C语言实现

```c
#include <stdio.h>

typedef struct Request Request;
struct Request {
    int type;
    int value;
};

typedef struct Handler Handler;
struct Handler {
    int type;
    Handler *next;
    void (*handle)(Handler *, Request *);
};

void handle_request(Handler *h, Request *r) {
    if (h->type == r->type) {
        printf("Handler %d handled request with value %d\n", h->type, r->value);
    } else if (h->next != NULL) {
        h->next->handle(h->next, r);
    } else {
        printf("No handler could handle request of type %d\n", r->type);
    }
}

int main() {
    Request r1 = { .type = 1, .value = 5 };
    Request r2 = { .type = 2, .value = 10 };
    Request r3 = { .type = 3, .value = 15 };
    Handler h1 = { .type = 2, .next = NULL, .handle = handle_request };
    Handler h2 = { .type = 1, .next = &h1, .handle = handle_request };
    h2.handle(&h2, &r1);
    h2.handle(&h2, &r2);
    h2.handle(&h2, &r3);
    return 0;
}

```

每个`Handler`对象都有一个指向下一个`Handler`对象的指针, 并且都有一个`handle()`函数。当请求`(Request)`到达时, 第一个`Handler`对象会尝试处理它, 如果不能处理, 就会把请求传递给下一个`Handler`对象, 直到有一个对象能够处理它为止
