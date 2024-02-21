---
layout: post
title: 设计模式（二十）-- 策略模式
description: 设计模式（二十）-- 策略模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 策略模式

## 1.概述

定义一系列算法类，将每一个算法封装起来，并让它们可以相互替换，策略模式让算法独立于使用它的客户而变化，也称为政策模式(Policy)。策略模式是一种对象行为型模式

## 2.结构图

![策略模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E7%AD%96%E7%95%A5%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Context（环境类）：环境类是使用算法的角色，它在解决某个问题（即实现某个方法）时可以采用多种策略。在环境类中维持一个对抽象策略类的引用实例，用于定义所采用的策略。
  
- Strategy（抽象策略类）：它为所支持的算法声明了抽象方法，是所有策略类的父类，它可以是抽象类或具体类，也可以是接口。环境类通过抽象策略类中声明的方法在运行时调用具体策略类中实现的算法。
  
- ConcreteStrategy（具体策略类）：它实现了在抽象策略类中声明的算法，在运行时，具体策略类将覆盖在环境类中定义的抽象策略类对象，使用一种具体的算法实现某个业务处理

## 4.C语言实现

```c
#include <stdio.h>

// 定义策略接口
typedef struct strategy_s {
  void (*execute)(void);
} strategy_t;

// 实现具体策略
void strategy_1(void) {
  printf("Executing strategy 1\n");
}

void strategy_2(void) {
  printf("Executing strategy 2\n");
}

// 定义上下文
typedef struct context_s {
  strategy_t *strategy;
} context_t;

// 设置上下文的策略
void set_strategy(context_t *context, strategy_t *strategy) {
  context->strategy = strategy;
}

// 执行上下文的策略
void execute_strategy(context_t *context) {
  context->strategy->execute();
}

int main() {
  strategy_t strategy_1_func = { strategy_1 };
  strategy_t strategy_2_func = { strategy_2 };
  context_t context;

  set_strategy(&context, &strategy_1_func);
  execute_strategy(&context);

  set_strategy(&context, &strategy_2_func);
  execute_strategy(&context);

  return 0;
}

```

