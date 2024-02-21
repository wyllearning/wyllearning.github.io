---
layout: post
title: 设计模式（九）-- 外观模式
description: 设计模式（九）-- 外观模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 外观模式

## 1.概述

为子系统中的一组接口提供一个统一的入口。外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用

## 2.结构图

![外观模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E5%A4%96%E8%A7%82%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Facade（外观角色）：在客户端可以调用它的方法，在外观角色中可以知道相关的（一个或者多个）子系统的功能和责任；在正常情况下，它将所有从客户端发来的请求委派到相应的子系统去，传递给相应的子系统对象处理。

- SubSystem（子系统角色）：在软件系统中可以有一个或者多个子系统角色，每一个子系统可以不是一个单独的类，而是一个类的集合，它实现子系统的功能；每一个子系统都可以被客户端直接调用，或者被外观角色调用，它处理由外观类传过来的请求；子系统并不知道外观的存在，对于子系统而言，外观角色仅仅是另外一个客户端而已。

## 4.C语言实现

```c
//subsystem1 class
typedef struct Subsystem1 Subsystem1;
struct Subsystem1 {
    void (*operation1)(Subsystem1*);
    void (*operation2)(Subsystem1*);
};

void operation1(Subsystem1* s){
    printf("operation1 of Subsystem1 is called\n");
}

void operation2(Subsystem1* s){
    printf("operation2 of Subsystem1 is called\n");
}

Subsystem1* new_Subsystem1(){
    Subsystem1* s = (Subsystem1*)malloc(sizeof(Subsystem1));
    s->operation1 = operation1;
    s->operation2 = operation2;
    return s;
}

//subsystem2 class
typedef struct Subsystem2 Subsystem2;
struct Subsystem2 {
    void (*operation1)(Subsystem2*);
    void (*operation2)(Subsystem2*);
    void (*operation3)(Subsystem2*);
};

void operation1_subsystem2(Subsystem2* s){
    printf("operation1 of Subsystem2 is called\n");
}

void operation2_subsystem2(Subsystem2* s){
    printf("operation2 of Subsystem2 is called\n");
}

void operation3_subsystem2(Subsystem2* s){
    printf("operation3 of Subsystem2 is called\n");
}

Subsystem2* new_Subsystem2(){
    Subsystem2* s = (Subsystem2*)malloc(sizeof(Subsystem2));
    s->operation1 = operation1_subsystem2;
    s->operation2 = operation2_subsystem2;
    s->operation3 = operation3_subsystem2;
    return s;
}

typedef struct Facade Facade;
struct Facade{
    Subsystem1* subsystem1;
    Subsystem2* subsystem2;
    void (*operation1)(Facade*);
    void (*operation2)(Facade*);
    void (*operation3)(Facade*);
    void (*operation4)(Facade*);
};

void operation1(Facade* f) {
  f->subsystem1->operation1(f->subsystem1);
}
void operation2(Facade* f) {
  f->subsystem1->operation1(f->subsystem1);
  f->subsystem2->operation2(f->subsystem2);
}

void operation3(Facade* f) {
  f->subsystem2->operation3(f->subsystem2);
}

void operation4(Facade* f) {
  f->subsystem1->operation2(f->subsystem1);
  f->subsystem2->operation1(f->subsystem2);
  f->subsystem2->operation2(f->subsystem2);
}

Facade* new_Facade(){
  Facade* f = (Facade*)malloc(sizeof(Facade));
  f->subsystem1 = new_Subsystem1();
  f->subsystem2 = new_Subsystem2();
  f->operation1 = operation1;
  f->operation2 = operation2;
  f->operation3 = operation3;
  f->operation4 = operation4;
  return f;
}

int main() {
  Facade* facade = new_Facade();
  facade->operation1(facade);
  facade->operation2(facade);
  facade->operation3(facade);
  facade->operation4(facade);
  free(facade->subsystem1);
  free(facade->subsystem2);
  free(facade);
  return 0;
}
```

