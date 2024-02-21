---
layout: post
title: 设计模式选择指南
description: 设计模式选择指南
categories:
  - 设计模式
tags:
  - 设计模式
---

# 设计模式选择指南第一版

## 一、前言

描述在实际的编程过程中如何选择相应的设计模式，并且给出了对应设计模式的C语言实现代码。

## 二、使用范围

| 设计模式     | 简述                                                         | 使用范围                                                     | 说明                                   |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------------- |
| 工厂模式     | 当你需要什么，只需要传入一个正确的参数，就可以获取你所需要的对象，而无须知道其创建细节 | 需要创建多个相似的对象，但是并不知道具体的类型，或者希望将对象的创建过程延迟到子类中进行 | 创建对象                               |
| 迭代器模式   | 提供一种方法来访问聚合对象，而不用暴露这个对象的内部表示，其别名为游标 | 需要提供一种方法顺序访问一个聚合对象中的各个元素，而不暴露其内部的表示，以及在不改变聚合对象的情况下支持对其元素进行遍历 | 遍历对象                               |
| 观察者模式   | 每当一个对象状态发生改变时，其相关依赖对象皆得到通知并被自动更新 | 需要在不同对象之间建立一对多的依赖关系，当一个对象更改状态时，它的所有依赖对象都会收到通知并自动更新 | 单个对象通知多个对象                   |
| 外观模式     | 为子系统中的一组接口提供一个统一的入口。外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用 | 当你需要为复杂系统提供一个简化的接口，以方便让客户端代码更容易地使用系统时 | 提供统一调用接口，类似于餐馆服务员     |
| 单例模式     | 确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例 | 1. 只需要一个全局对象，保证不会被多次实例化。 2. 对于频繁使用且耗费资源较多的对象。 3. 在系统只需要一个实例的情况下，如日志记录器。 | 只存在一个对象                         |
| 适配器模式   | 将一个接口转换成客户希望的另一个接口，使接口不兼容的那些类可以一起工作 | 1. 当需要使用一个已有的类，但其接口与当前系统不兼容时。 2. 当需要创建一个可重用的类，该类可将不兼容的接口转换为目标接口时。  3. 当需要使用一些独立的类，但是不能修改它们的代码时 4. 当需要组合多个对象的功能时，适配器可以将这些功能组合到一起，并作为单个接口提供给客户端。 | 兼容原本不兼容的接口                   |
| 模板方法模式 | 定义一个操作中算法的框架，而将一些步骤延迟到子类中           | 1. 当多个子类有共同的方法，并且逻辑基本相同时。 2. 需要控制子类执行次序的情况。 3. 需要在不改变算法结构情况下，重新定义算法中的某些步骤。 | 实现一个步骤，通过子类重写部分父类方法 |
| 职责链模式   | 免请求发送者与接收者耦合在一起，让多个对象都有可能接收请求，将这些对象连接成一条链，并且沿着这条链传递请求，直到有对象处理它为止 | 适用于在多个对象之间传递请求，通过每个对象依次处理该请求，直到得到符合要求的响应。特别适用于需要动态指定处理者，或者请求者和处理者之间的关系紧密而复杂的情况 | 请求链式处理                           |
| 中介者模式   | 用一个中介对象（中介者）来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互 | 1. 多个对象间存在复杂的相互依赖关系，需要减少这种依赖关系。 2. 对象间通信复杂，需要抽象出一个中介者对象来简化通信。 3. 需要在对象间动态地指定、撤销协调关系。 4. 对象的行为分别独立，但却有联系，需要把它们的行为集中在一起处理。 | 协调多个对象之间的交互，类似QQ群       |
| 桥接模式     | 将抽象部分与它的实现部分分离，使它们都可以独立地变化         | 1. 当你想要独立地改变或扩展类的实现和它的抽象 2. 当一个类存在多维变化，并且你不想使用多重继承或类继承 3. 当你想要在多个类层次结构中分离抽象接口和实现。 | 处理多维度变化                         |
| 组合模式     | 组合多个对象形成树形结构以表示具有“整体—部分”关系的层次结构。组合模式对单个对象（即叶子对象）和组合对象（即容器对象）的使用具有一致性，组合模式又可以称为“整体—部分 | 需要表示对象的部分-整体层次结构，以表示“整体/部分”关系，允许用户以相同的方式处理单独对象和组合对象 | 树形结构，部分-整体的关系              |
| 代理模式     | 给某一个对象提供一个代理或占位符，并由代理对象来控制对原对象的访问 | 1. 访问控制：控制对资源的访问。 2. 远程代理：隐藏远程资源的存在，并且可以在本地做一些优化。 3. 虚拟代理：控制创建开销大的资源的创建。 4. 缓存代理：为开销大的资源提供本地缓存。 5. 保护代理：控制对原始对象的访问。 | 了解不多                               |
| 命令模式     | 将一个请求封装为一个对象，从而让我们可用不同的请求对客户进行参数化；对请求排队或者记录请求日志，以及支持可撤销的操作 | 1. 记录请求：支持撤销操作。 2. 将请求封装成对象，从而可以使用不同的请求对客户进行参数化。 3. 允许请求的一方和接收请求的一方解耦，从而使得请求的一方不必知道接收请求的一方是谁，接收请求的一方不必知道请求的一方是谁。 4. 通过使用命令队列，实现请求的排队和执行。 5. 支持撤销操作，即支持取消最后一次请求。 | 请求封装为对象                         |
| 策略模式     | 定义一系列算法类，将每一个算法封装起来，并让它们可以相互替换，策略模式让算法独立于使用它的客户而变化 | 1. 需要算法的不同版本，并且可以在运行时动态选择它们。 2. 需要将不同的算法封装到独立的对象中，以便于使用和交换。 3. 需要避免多个条件语句，将算法的选择委托给独立的策略类。 | 通过不同的算法实现某一功能             |
| 状态模式     | 允许一个对象在其内部状态改变时改变它的行为，对象看起来似乎修改了它的类 | 1. 对象的行为是由它的状态改变引起的。 2. 一个对象具有很多种状态，每种状态对应不同的行为。 3. 你需要将一个对象的多种行为分散到不同的状态对象中，而不是将它们的行为都写在一个类中。 | 改变状态后相应的方法也改变             |
| 装饰模式     | 动态地给一个对象增加一些额外的职责                           | 1. 在不改变原有对象的基础上，动态地给该对象添加新的职责； 2. 需要扩展一个类的功能，或给一个类添加附加责任。 | 动态给类额外增加方法                   |
| 原型模式     | 使用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象 | 1. 对象创建非常复杂或者有很多参数； 2. 对象的类型在运行时不能确定； 3. 需要动态地创建对象的副本。 | 对象的克隆                             |
| 建造者模式   | 将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示 | 1. 对象具有多个复杂的内部组件； 2. 对象的创建过程必须独立于对象的表示； 3. 需要支持对象的逐步创建。 | 复杂的类分步创建                       |
| 备忘录模式   | 在不破坏封装的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，这样可以在以后将对象恢复到原先保存的状态 | 1. 保存一个对象在某一个时刻的全部状态或部分状态，这样以后需要时它能够恢复到先前的状态，实现撤销操作。 2. 防止外界对象破坏一个对象历史状态的封装性，避免将对象历史状态的实现细节暴露给外界对象。 | 撤销功能                               |
| 解释器模式   | 定义一个语言的文法，并且建立一个解释器来解释该语言中的句     | 1.可以将一个需要解释执行的语言中的句子表示为一个抽象语法树。 2. 一些重复出现的问题可以用一种简单的语言来进行表达。 3. 一个语言的文法较为简单。 4. 执行效率不是关键问题。 | 自定义语言的实现，基本用不到           |
| 访问者模式   | 提供一个作用于某对象结构中的各元素的操作表示，它使我们可以在不改变各元素的类的前提下定义作用于这些元素的新操作 | 需要对一个对象结构中的对象进行很多不同的并且不相关的操作，而需要避免让这些操作"污染"这些对象的类，使用访问者模式将这些封装到类中 | 操作复杂对象结构，不污染对象           |
| 享元模式     | 运用共享技术有效地支持大量细粒度对象的复用                   | 1. 一个系统有大量相同或者相似的对象，造成内存的大量耗费。 2. 对象的大部分状态都可以外部化，可以将这些外部状态传入对象中。 3. 在使用享元模式时需要维护一个存储享元对象的享元池，而这需要耗费一定的系统资源，因此，应当在需要多次重复使用享元对象时才值得使用享元模式。 | 共享类                                 |

## 三、C语言实现

### 1.工厂模式

由于C语言并非面向对象语言，需要用到的面向对象特性并不完全，因此一般只使用简单工厂模式

```c
#include <stdio.h>

// 定义产品接口
typedef struct {
    const char* type;
    void (*print)();
} Product;

// 具体产品1
typedef struct {
    Product product;
    int value;
} ConcreteProductA;

// 具体产品2
typedef struct {
    Product product;
    int value;
} ConcreteProductB;

// 实现产品函数
void printA() {
    printf("I am Product A\n");
}

void printB() {
    printf("I am Product B\n");
}

// 简单工厂函数
Product* createProduct(const char* type, int value) {
    if (strcmp(type, "A") == 0) {
        ConcreteProductA* productA = (ConcreteProductA*)malloc(sizeof(ConcreteProductA));
        productA->product.type = "A";
        productA->product.print = printA;
        productA->value = value;
        return (Product*)productA;
    } else if (strcmp(type, "B") == 0) {
        ConcreteProductB* productB = (ConcreteProductB*)malloc(sizeof(ConcreteProductB));
        productB->product.type = "B";
        productB->product.print = printB;
        productB->value = value;
        return (Product*)productB;
    }
    return NULL;
}

int main() {
    Product* productA = createProduct("A", 5);
    productA->print(); // 输出: "I am Product A"
    Product* productB = createProduct("B", 10);
    productB->print(); // 输出: "I am Product B"
    return 0;
}
```

### 2.单例模式

饿汉单例模式，在程序运行前就创建对象

```c
#include <stdio.h>

// 定义单例结构体
struct Singleton {
    int data;
};

// 静态实例
static struct Singleton instance = { .data = 0 };

// 获取实例的函数
struct Singleton* get_instance() {
    return &instance;
}

int main(int argc, char** argv) {
    struct Singleton* s1 = get_instance();
    struct Singleton* s2 = get_instance();
    printf("s1->data = %d\n", s1->data);
    printf("s2->data = %d\n", s2->data);
    return 0;
}
```

懒汉单例模式，调用时才创建，为了避免多线程同时调用，每次调用需要判断是指针否为空

```c
#include <stdio.h>

typedef struct {
    int value;
} Singleton;

Singleton* getInstance() {
    static Singleton *instance = NULL;
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

### 3.原型模式

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

### 4.适配器模式

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

### 5.组合模式

```c
#include <stdio.h>

typedef struct Component Component;
struct Component{
    void (*Add)(Component*, Component*);
    void (*Remove)(Component*, Component*);
    Component* (*GetChild)(Component*, int);
    void (*Operation)(Component*);
    Component* parent;
    Component* child_head;
};

// 定义叶子节点
typedef struct Leaf Leaf;
struct Leaf{
    Component component;
};

// 定义容器节点
typedef struct Composite Composite;
struct Composite{
    Component component;
    char* name;
};

// 叶子节点
void LeafOperation(Component* self){
    printf("Leaf operation\n");
}

// 容器节点
void CompositeOperation(Component* self){
    Composite* composite = (Composite*)self;
    printf("Composite %s operation\n", composite->name);
    Component* child = composite->component.child_head;
    while(child != NULL) {
        child->Operation(child);
        child = child->parent;
    }
}

void Add(Component* self, Component* component) {
    if(self != NULL && component != NULL) {
        component->parent = self->child_head;
        self->child_head = component;
    }
}

void Remove(Component* self, Component* component){
    if(self != NULL && component != NULL) {
        Component* current = self->child_head;
        if(current == component) {
            self->child_head = current->parent;
        } else {
            while(current != NULL) {
                if(current->parent == component) {
                    current->parent = component->parent;
                    break;
                }
                current = current->parent;
            }
        }
    }
}

Component* GetChild(Component* self, int index){
    if(self != NULL) {
        Component* current = self->child_head;
        for(int i = 0; current != NULL && i < index; i++) {
            current = current->parent;
        }
        return current;
    }
    return NULL;
}

int main(void) {
    Composite root;
    root.component.Add = Add;
    root.component.Remove = Remove;
    root.component.GetChild = GetChild;
    root.component.Operation = CompositeOperation;
    root.name = "root";
    Leaf leaf1;
    leaf1.component.Operation = LeafOperation;
    root.component.Add(&root.component, &leaf1.component);
    Composite comp1;
    comp1.component.Add = Add;
    comp1.component.Remove = Remove;
    comp1.component.GetChild = GetChild;
    comp1.component.Operation = CompositeOperation;
    comp1.name = "comp1";
    root.component.Add(&root.component, &comp1.component);
    Leaf leaf2;
    leaf2.component.Operation = LeafOperation;
    comp1.component.Add(&comp1.component, &leaf2.component);
    root.component.Operation(&root.component);
    return 0;
}
```

### 6.桥接模式

```c
#include <stdio.h>

// 类
typedef struct TV TV;
struct TV{
  void (*turnOn)(TV*);
  void (*turnOff)(TV*);
  void (*setChannel)(TV*, int);
  void (*setVolume)(TV*, int);
};

typedef struct {
    TV tv;
    int channel;
    int volume;
} SonyTV;

typedef struct {
    TV tv;
    int channel;
    int volume;
} SamsungTV;

// 方法
void SonyTV_turnOn(TV* this){
    printf("Sony TV is turned on\n");
}

void SonyTV_turnOff(TV* this){
    printf("Sony TV is turned off\n");
}

void SonyTV_setChannel(TV* this, int channel){
    SonyTV* self = (SonyTV*) this;
    self->channel = channel;
    printf("Sony TV channel set to %d\n", channel);
}

void SonyTV_setVolume(TV* this, int volume){
    SonyTV* self = (SonyTV*) this;
    self->volume = volume;
    printf("Sony TV volume set to %d\n", volume);
}

void SamsungTV_turnOn(TV* this){
    printf("Samsung TV is turned on\n");
}

void SamsungTV_turnOff(TV* this){
    printf("Samsung TV is turned off\n");
}

void SamsungTV_setChannel(TV* this, int channel){
    SamsungTV* self = (SamsungTV*) this;
    self->channel = channel;
    printf("Samsung TV channel set to %d\n", channel);
}

void SamsungTV_setVolume(TV* this, int volume){
    SamsungTV* self = (SamsungTV*) this;
    self->volume = volume;
    printf("Samsung TV volume set to %d\n", volume);
}

// 调用
int main(void) {
    SonyTV SonyTV1;
    SonyTV1.tv.turnOn = SonyTV_turnOn;
    SonyTV1.tv.turnOff = SonyTV_turnOff;
    SonyTV1.tv.setChannel = SonyTV_setChannel;
    SonyTV1.tv.setVolume = SonyTV_setVolume;

    SamsungTV SamsungTV1;
    SamsungTV1.tv.turnOn = SamsungTV_turnOn;
    SamsungTV1.tv.turnOff = SamsungTV_turnOff;
    SamsungTV1.tv.setChannel = SamsungTV_setChannel;
    SamsungTV1.tv.setVolume = SamsungTV_setVolume;

    SonyTV1.tv.turnOn(&SonyTV1.tv);
    SonyTV1.tv.setChannel(&SonyTV1.tv, 5);
    SonyTV1.tv.setVolume(&SonyTV1.tv, 20);
    SonyTV1.tv.turnOff(&SonyTV1.tv);

    SamsungTV1.tv.turnOn(&SamsungTV1.tv);
    SamsungTV1.tv.setChannel(&SamsungTV1.tv, 10);
    SamsungTV1.tv.setVolume(&SamsungTV1.tv, 30);
    SamsungTV1.tv.turnOff(&SamsungTV1.tv);
    return 0;
}
```

### 7.建造者模式

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

### 8.装饰模式

```c
#include <stdio.h>

// 基础组件
typedef struct Component {
    void (*operation)();
} Component;

void operation() {
    printf("Component operation\n");
}

// 装饰组件A
typedef struct DecoratorA {
    Component base;
    void (*addedBehaviorA)();
} DecoratorA;

void addedBehaviorA() {
    printf("Added behavior A\n");
}

void operationA(Component* base) {
    base->operation();
    addedBehaviorA();
}

// 装饰组件B
typedef struct DecoratorB {
    Component base;
    void (*addedBehaviorB)();
} DecoratorB;

void addedBehaviorB() {
    printf("Added behavior B\n");
}

void operationB(Component* base) {
    base->operation();
    addedBehaviorB();
}

// 装饰组件C
typedef struct DecoratorC {
    Component base;
    void (*addedBehaviorC)();
} DecoratorC;

void addedBehaviorC() {
    printf("Added behavior C\n");
}

void operationC(Component* base) {
    base->operation();
    addedBehaviorC();
}

int main() {
    Component component = { &operation };
    DecoratorA decoratorA = { { &operationA }, &addedBehaviorA };
    DecoratorB decoratorB = { { &operationB }, &addedBehaviorB };
    DecoratorC decoratorC = { { &operationC }, &addedBehaviorC };

    // 基础组件
    component.operation();
    printf("\n");

    // 装饰组件A
    decoratorA.base.operation = &operationA;
    ((Component*)&decoratorA)->operation(&component);
    printf("\n");

    // 装饰组件B
    decoratorB.base.operation = &operationB;
    ((Component*)&decoratorB)->operation((Component*)&decoratorA);
    printf("\n");

    // 装饰组件C
    decoratorC.base.operation = &operationC;
    ((Component*)&decoratorC)->operation((Component*)&decoratorB);
    return 0;
}
```

### 9.外观模式

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

### 10.享元模式

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_CACHE_SIZE 100

// 享元类型
typedef struct {
    char *value;
    int refCount;
} Flyweight;

// 享元工厂类型
typedef struct {
    Flyweight flyweights[MAX_CACHE_SIZE];
    int totalFlyweights;
} FlyweightFactory;

// 初始化享元工厂
void initFlyweightFactory(FlyweightFactory *factory) {
    factory->totalFlyweights = 0;
}

// 获取一个享元
Flyweight *getFlyweight(FlyweightFactory *factory, char *value) {
    for (int i = 0; i < factory->totalFlyweights; i++) {
        if (strcmp(factory->flyweights[i].value, value) == 0) {
            factory->flyweights[i].refCount++;
            return &factory->flyweights[i];
        }
    }
    // 如果没有找到相同的享元，则创建一个新的享元
    Flyweight *newFlyweight = &factory->flyweights[factory->totalFlyweights++];
    newFlyweight->value = strdup(value);
    newFlyweight->refCount = 1;
    return newFlyweight;
}

// 释放一个享元
void releaseFlyweight(FlyweightFactory *factory, Flyweight *flyweight) {
    for (int i = 0; i < factory->totalFlyweights; i++) {
        if (&factory->flyweights[i] == flyweight) {
            if (--flyweight->refCount == 0) {
                free(flyweight->value);
                memmove(&factory->flyweights[i], &factory->flyweights[i+1], sizeof(Flyweight) * (factory->totalFlyweights - i - 1));
                factory->totalFlyweights--;
            }
            break;
        }
    }
}

int main() {
    FlyweightFactory factory;
    initFlyweightFactory(&factory);
    Flyweight *f1 = getFlyweight(&factory, "hello");
    Flyweight *f2 = getFlyweight(&factory, "world");
    Flyweight *f3 = getFlyweight(&factory, "hello");
    printf("f1: %p, value: %s, refCount: %d\n", f1, f1->value, f1->refCount);
    printf("f2: %p, value: %s, refCount: %d\n", f2, f2->value, f2->refCount);
    printf("f3: %p, value: %s, refCount: %d\n", f3, f3->value, f3->refCount);
    releaseFlyweight(&factory, f1);
    releaseFlyweight(&factory, f2);
    releaseFlyweight(&factory, f3);
    return 0;
}
```

### 11.代理模式

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

### 12.职责链模式

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

### 13.命令模式

```c
#include <stdio.h>

typedef struct command command;
struct command
{
    void (*execute)(command *);
    void (*undo)(command *);
};

typedef struct Light Light;
struct Light
{
    int state;
};

typedef struct light_on_command light_on_command;
struct light_on_command
{
    command command;
    Light *light;
};

void light_on(light_on_command *l)
{
    l->light->state = 1;
    printf("Light is on\n");
}

void light_off(light_on_command *l)
{
    l->light->state = 0;
    printf("Light is off\n");
}

light_on_command light_on_command_obj = {
    .command = {.execute = (void (*)(command *))light_on,
                .undo = (void (*)(command *))light_off},
    .light = &(Light){0}};

int main()
{
    light_on_command_obj.command.execute((command *)&light_on_command_obj);
    light_on_command_obj.command.undo((command *)&light_on_command_obj);
    return 0;
}

```

### 14.解释器模式

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

### 15.迭代器模式

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

### 16.中介者模式

```c
#include <stdio.h>

// 抽象组件
typedef struct Component Component;
struct Component
{
    void (*operation)(Component *);
};

// 具体组件
typedef struct ConcreteComponent ConcreteComponent;
struct ConcreteComponent
{
    Component component;
};

// 中介者
typedef struct Mediator Mediator;
struct Mediator
{
    void (*mediate)(Mediator *, Component *, Component *);
};

// 具体中介者
typedef struct ConcreteMediator ConcreteMediator;
struct ConcreteMediator
{
    Mediator mediator;
    ConcreteComponent *component1;
    ConcreteComponent *component2;
};

// 组件操作
void componentOperation(Component *component)
{
    printf("Component operation\n");
}

// 中介者操作
void mediate(Mediator *mediator, Component *component1, Component *component2)
{
    ConcreteMediator *m = (ConcreteMediator *)mediator;
    m->component1->component.operation(&(m->component1->component));
    m->component2->component.operation(&(m->component2->component));
}

int main()
{
    ConcreteComponent component1;
    component1.component.operation = componentOperation;

    ConcreteComponent component2;
    component2.component.operation = componentOperation;

    ConcreteMediator mediator;
    mediator.mediator.mediate = mediate;
    mediator.component1 = &component1;
    mediator.component2 = &component2;

    mediator.mediator.mediate(&mediator, &component1.component,
                              &component2.component);
    return 0;
}

```

### 17.备忘录模式

```c
#include <stdio.h>
#include <stdlib.h>

// 需要被记录状态的对象
typedef struct Originator Originator;
struct Originator
{
    int state;
};

// 备忘录对象
typedef struct Memento Memento;
struct Memento
{
    int state;
};

// 负责创建和恢复备忘录的对象
typedef struct CareTaker CareTaker;
struct CareTaker
{
    Memento *memento;
};

// 创建备忘录
Memento *createMemento(Originator *originator)
{
    Memento *m = (Memento *)malloc(sizeof(Memento));
    m->state = originator->state;
    return m;
}

// 恢复备忘录
void setMemento(Originator *originator, Memento *memento)
{
    originator->state = memento->state;
}

int main()
{
    Originator originator;
    originator.state = 10;

    // 创建备忘录
    CareTaker careTaker;
    careTaker.memento = createMemento(&originator);

    // 修改状态
    originator.state = 20;

    // 恢复状态
    setMemento(&originator, careTaker.memento);
    printf("Originator state: %d\n", originator.state);

    return 0;
}

```

### 18.观察者模式

```c
#include <stdio.h>
#include <stdlib.h>

// 观察者接口
typedef void (*update_func)(int);

// 主题
typedef struct
{
    int state;
    update_func *observers;
    int observer_count;
} Subject;

// 添加观察者
void subject_add_observer(Subject *s, update_func observer)
{
    s->observers =
        realloc(s->observers, sizeof(update_func) * (s->observer_count + 1));
    s->observers[s->observer_count++] = observer;
}

// 移除观察者
void subject_remove_observer(Subject *s, update_func observer)
{
    for (int i = 0; i < s->observer_count; i++)
    {
        if (s->observers[i] == observer)
        {
            s->observers[i] = s->observers[--s->observer_count];
            break;
        }
    }
}

// 通知所有观察者
void subject_notify(Subject *s)
{
    for (int i = 0; i < s->observer_count; i++)
    {
        s->observers[i](s->state);
    }
}

// 观察者1
void observer1(int state)
{
    printf("Observer 1: State changed to %d\n", state);
}

// 观察者2
void observer2(int state)
{
    printf("Observer 2: State changed to %d\n", state);
}

int main()
{
    Subject subject = {0};
    subject_add_observer(&subject, observer1);
    subject_add_observer(&subject, observer2);
    subject.state = 1;
    subject_notify(&subject);
    subject_remove_observer(&subject, observer1);
    subject.state = 2;
    subject_notify(&subject);
    return 0;
}

```

### 19.状态模式

```c
#include <stdio.h>

enum State
{
    STATE_A,
    STATE_B,
    NUM_STATES
};

struct Object
{
    const struct StateFunctions *state;
    int x;
};

struct StateFunctions
{
    void (*const func1)(struct Object *);
    void (*const func2)(struct Object *);
};

static void stateAFunc1(struct Object *obj)
{
    printf("State A, func1, x = %d\n", obj->x);
}

static void stateAFunc2(struct Object *obj)
{
    printf("State A, func2, x = %d\n", obj->x);
}

static void stateBFunc1(struct Object *obj)
{
    printf("State B, func1, x = %d\n", obj->x);
}

static void stateBFunc2(struct Object *obj)
{
    printf("State B, func2, x = %d\n", obj->x);
}

static const struct StateFunctions stateFunctions[NUM_STATES] = {
    {stateAFunc1, stateAFunc2},
    {stateBFunc1, stateBFunc2},
};

static void changeState(struct Object *obj, enum State newState)
{
    obj->state = &stateFunctions[newState];
}

int main()
{
    struct Object obj = {&stateFunctions[STATE_A], 0};
    obj.state->func1(&obj);
    obj.state->func2(&obj);
    changeState(&obj, STATE_B);
    obj.state->func1(&obj);
    obj.state->func2(&obj);

    return 0;
}
```

### 20.策略模式

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

### 21.模板方法模式

```c
#include <stdio.h>

// 抽象类
typedef struct AbstractClass AbstractClass;
struct AbstractClass
{
    void (*primitiveOperation1)(AbstractClass *);
    void (*primitiveOperation2)(AbstractClass *);
    void (*templateMethod)(AbstractClass *);
};

// 具体类
typedef struct ConcreteClass ConcreteClass;
struct ConcreteClass
{
    AbstractClass super;
};

// 具体类方法
void primitiveOperation1(AbstractClass *abstractClass)
{
    printf("具体类primitiveOperation1实现\n");
}

void primitiveOperation2(AbstractClass *abstractClass)
{
    printf("具体类primitiveOperation2实现\n");
}

void templateMethod(AbstractClass *abstractClass)
{
    printf("模板方法开始\n");
    abstractClass->primitiveOperation1(abstractClass);
    abstractClass->primitiveOperation2(abstractClass);
    printf("模板方法结束\n");
}

// 具体类初始化
void concreteClassInit(ConcreteClass *concreteClass)
{
    concreteClass->super.primitiveOperation1 = primitiveOperation1;
    concreteClass->super.primitiveOperation2 = primitiveOperation2;
    concreteClass->super.templateMethod = templateMethod;
}

int main()
{
    ConcreteClass concreteClass;
    concreteClassInit(&concreteClass);
    concreteClass.super.templateMethod(&concreteClass.super);
    return 0;
}
```

### 22.访问者模式

```c
#include <stdio.h>

// 声明抽象元素类型
typedef struct Element Element;
struct Element {
  void (*Accept)(Element *self, void *visitor);
};

// 声明抽象访问者类型
typedef struct Visitor Visitor;
struct Visitor {
  void (*VisitA)(Visitor *self, Element *elementA);
  void (*VisitB)(Visitor *self, Element *elementB);
};

// 元素A
typedef struct ElementA ElementA;
struct ElementA {
  Element base;
  int valueA;
};

void ElementA_Accept(Element *self, void *visitor) {
  Visitor *v = visitor;
  v->VisitA(v, (ElementA *) self);
}

// 元素B
typedef struct ElementB ElementB;
struct ElementB {
  Element base;
  int valueB;
};

void ElementB_Accept(Element *self, void *visitor) {
  Visitor *v = visitor;
  v->VisitB(v, (ElementB *) self);
}

// 访问者
typedef struct ConcreteVisitor ConcreteVisitor;
struct ConcreteVisitor {
  Visitor base;
};

void ConcreteVisitor_VisitA(Visitor *self, ElementA *elementA) {
  printf("Visiting ElementA with value %d\n", elementA->valueA);
}

void ConcreteVisitor_VisitB(Visitor *self, ElementB *elementB) {
  printf("Visiting ElementB with value %d\n", elementB->valueB);
}

int main() {
  ElementA a = {
    { ElementA_Accept },
    10
  };
  
  ElementB b = {
    { ElementB_Accept },
    20
  };

  ConcreteVisitor v = {
    { ConcreteVisitor_VisitA, ConcreteVisitor_VisitB }
  };

  a.base.Accept((Element *) &a, (Visitor *) &v);
  b.base.Accept((Element *) &b, (Visitor *) &v);

  return 0;
}

```

