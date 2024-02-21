---
layout: post
title: 设计模式（十四）-- 解释器模式
description: 设计模式（十四）-- 解释器模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 解释器模式

## 1.概述

定义一个语言的文法，并且建立一个解释器来解释该语言中的句子，这里的“语言”是指使用规定格式和语法的代码。解释器模式是一种类行为型模式

## 2.结构图

![解释器模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E8%A7%A3%E9%87%8A%E5%99%A8%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- AbstractExpression（抽象表达式）：在抽象表达式中声明了抽象的解释操作，它是所有终结符表达式和非终结符表达式的公共父类。

- TerminalExpression（终结符表达式）：终结符表达式是抽象表达式的子类，它实现了与文法中的终结符相关联的解释操作，在句子中的每一个终结符都是该类的一个实例。通常在一个解释器模式中只有少数几个终结符表达式类，它们的实例可以通过非终结符表达式组成较为复杂的句子。
  
- NonterminalExpression（非终结符表达式）：非终结符表达式也是抽象表达式的子类，它实现了文法中非终结符的解释操作，由于在非终结符表达式中可以包含终结符表达式，也可以继续包含非终结符表达式，因此其解释操作一般通过递归的方式来完成。
  
- Context（环境类）：环境类又称为上下文类，它用于存储解释器之外的一些全局信息，通常它临时存储了需要解释的语句。


## 4.C语言实现

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_EXPRESSION_LEN 100

typedef struct Context Context;
struct Context
{
    char expression[MAX_EXPRESSION_LEN];
    int pos;
};

typedef struct AbstractExpression AbstractExpression;
struct AbstractExpression
{
    void (*interpret)(AbstractExpression *, Context *);
};

typedef struct TerminalExpression TerminalExpression;
struct TerminalExpression
{
    AbstractExpression expression;
    char *token;
};

void interpret_terminal(TerminalExpression *e, Context *c)
{
    char *token = e->token;
    int len = strlen(token);
    if (strncmp(c->expression + c->pos, token, len) == 0)
    {
        c->pos += len;
        printf("Matched %s\n", token);
    }
    else
    {
        printf("Error: %s not found at position %d\n", token, c->pos);
        exit(1);
    }
}

typedef struct NonTerminalExpression NonTerminalExpression;
struct NonTerminalExpression
{
    AbstractExpression expression;
    AbstractExpression *expression1;
    AbstractExpression *expression2;
};

void interpret_non_terminal(NonTerminalExpression *e, Context *c)
{
    e->expression1->interpret(e->expression1, c);
    e->expression2->interpret(e->expression2, c);
}

int main()
{
    Context c = {.expression = "1+2", .pos = 0};
    TerminalExpression t1 = {
        .expression = {.interpret = (void (*)(AbstractExpression *,
                                              Context))interpret_terminal},
        .token = "1"};
    TerminalExpression t2 = {
        .expression = {.interpret = (void()(AbstractExpression *,
                                            Context))interpret_terminal},
        .token = "+"};
    TerminalExpression t3 = {
        .expression = {.interpret = (void()(AbstractExpression *,
                                            Context *))interpret_terminal},
        .token = "2"};

    NonTerminalExpression e = {
        .expression = {.interpret = (void (*)(AbstractExpression *, Context *))
                           interpret_non_terminal},
        .expression1 = (AbstractExpression *)&t1,
        .expression2 = (AbstractExpression *)&t2};
    NonTerminalExpression e2 = {
        .expression = {.interpret = (void (*)(AbstractExpression *, Context *))
                           interpret_non_terminal},
        .expression1 = (AbstractExpression *)&e,
        .expression2 = (AbstractExpression *)&t3};
    e2.expression.interpret((AbstractExpression *)&e2, &c);
    return 0;
}
```

在这个实现中, 我们有一个抽象表达式(AbstractExpression)和一个终结符表达式(TerminalExpression)和非终结符表达式(NonTerminalExpression)。终结符表达式可以是单个的字符或字符串, 非终结符表达式则是由其他表达式组成的复合表达式

在main函数中, 我们使用了三个终结符表达式来表示数字1, 加号和数字2, 然后使用两个非终结符表达式来组合这些终结符表达式, 从而表示整个表达式"1+2".

需要注意的是，这个实现缺少许多实际应用中常用的功能，例如语法树、语法分析器等。在C语言中实现解释器模式可能会有些困难
