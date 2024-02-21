---
layout: post
title: 设计模式（二）-- 单例模式
description: 设计模式（二）-- 单例模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 单例模式

## 一、单例模式

### 1.概述

1. 构造函数为private

2. 定义私有类型的成员变量

3. 通过共有的静态方法实例化该成员变量

   简单代码如下

   ```c
   #include <stdio.h>
   
   typedef struct {
       int value;
   } Singleton;
   
   Singleton* getInstance() {
       static Singleton instance = {0};
       return &instance;
   }
   
   int main() {
       Singleton* singleton1 = getInstance();
       singleton1->value = 5;
       printf("singleton1 value: %d\n", singleton1->value); // 输出: "singleton1 value: 5"
   
       Singleton* singleton2 = getInstance();
       printf("singleton2 value: %d\n", singleton2->value); // 输出: "singleton2 value: 5"
       return 0;
   }
   
   ```

   

### 2.结构图

![单例模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F.png)

### 3.饿汉式单例类

```c
#include <stdio.h>

typedef struct {
    int value;
} Singleton;

Singleton* getInstance() {
    static Singleton instance = {0};
    return &instance;
}

int main() {
    Singleton* singleton1 = getInstance();
    singleton1->value = 5;
    printf("singleton1 value: %d\n", singleton1->value); // 输出: "singleton1 value: 5"

    Singleton* singleton2 = getInstance();
    printf("singleton2 value: %d\n", singleton2->value); // 输出: "singleton2 value: 5"
    return 0;
}
```

其结构图如下

![饿汉单例模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E9%A5%BF%E6%B1%89%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F.png)

### 4.懒汉单例模式

懒汉式单例在第一次调用getInstance()方法时实例化，在类加载时并不自行实例化，这种技术又称为延迟加载(Lazy Load)技术，即需要的时候再加载实例，为了避免多个线程同时调用getInstance()方法

```c
#include <stdio.h>

typedef struct {
    int value;
} Singleton;

Singleton* getInstance() {
    static Singleton *instance;
    if(instance == NULL)
    {
        instance = (Singleton *)malloc(sizeof(Singleton));
    }
    return instance;
}

int main() {
    Singleton* singleton1 = getInstance();
    singleton1->value = 5;
    printf("singleton1 value: %d\n", singleton1->value); // 输出: "singleton1 value: 5"

    Singleton* singleton2 = getInstance();
    printf("singleton2 value: %d\n", singleton2->value); // 输出: "singleton2 value: 5"
    return 0;
}
```

