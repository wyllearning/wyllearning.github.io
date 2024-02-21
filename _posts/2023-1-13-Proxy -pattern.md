---
layout: post
title: 设计模式（十一）-- 代理模式
description: 设计模式（十一）-- 代理模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 代理模式

## 1.概述

给某一个对象提供一个代理或占位符，并由代理对象来控制对原对象的访问

## 2.结构图

![代理模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Subject（抽象主题角色）：它声明了真实主题和代理主题的共同接口，这样一来在任何使用真实主题的地方都可以使用代理主题，客户端通常需要针对抽象主题角色进行编程。

- Proxy（代理主题角色）：它包含了对真实主题的引用，从而可以在任何时候操作真实主题对象；在代理主题角色中提供一个与真实主题角色相同的接口，以便在任何时候都可以替代真实主题；代理主题角色还可以控制对真实主题的使用，负责在需要的时候创建和删除真实主题对象，并对真实主题对象的使用加以约束。通常，在代理主题角色中，客户端在调用所引用的真实主题操作之前或之后还需要执行其他操作，而不仅仅是单纯调用真实主题对象中的操作。
  
- RealSubject（真实主题角色）：它定义了代理角色所代表的真实对象，在真实主题角色中实现了真实的业务操作，客户端可以通过代理主题角色间接调用真实主题角色中定义的操作。
  



## 4.C语言实现

```c
// 接口
struct Interface {
    void (*func1)();
    void (*func2)();
    void (*func3)();
};

// 代理
struct Proxy {
    struct Interface* interface;
    int field1;
    int field2;
};

void func1() {
    // 目标对象的具体实现
}

void func2() {
    // 目标对象的具体实现
}

void func3() {
    // 目标对象的具体实现
}

void proxy_func1(struct Proxy* proxy) {
    proxy->interface->func1();
}

void proxy_func2(struct Proxy* proxy) {
    proxy->interface->func2();
}

void proxy_func3(struct Proxy* proxy) {
    proxy->interface->func3();
}


int main() {
    struct Interface interface = { func1, func2, func3 };
    struct Proxy proxy = { &interface, 0, 0 };
    proxy_func1(&proxy);
    proxy_func2(&proxy);
    proxy_func3(&proxy);
    return 0;
}

```

