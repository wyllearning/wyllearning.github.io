---
layout: post
title: 设计模式（五）-- 组合模式
description: 设计模式（五）-- 组合模式
categories:
  - 设计模式
tags:
  - 设计模式
  - 面向对象
---

# 组合模式

## 1.概述

组合多个对象形成树形结构以表示具有“整体—部分”关系的层次结构。组合模式对单个对象（即叶子对象）和组合对象（即容器对象）的使用具有一致性，组合模式又可以称为“整体—部分”(Part-Whole)模式，它是一种对象结构型模式

## 2.结构图

![组合模式](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E7%BB%84%E5%90%88%E6%A8%A1%E5%BC%8F.png)

## 3.角色

- Component（抽象构件）：它可以是接口或抽象类，为叶子构件和容器构件对象声明接口，在该角色中可以包含所有子类共有行为的声明和实现。在抽象构件中定义了访问及管理它的子构件的方法，如增加子构件、删除子构件、获取子构件等。

- Leaf（叶子构件）：它在组合结构中表示叶子节点对象，叶子节点没有子节点，它实现了在抽象构件中定义的行为。对于那些访问及管理子构件的方法，可以通过异常等方式进行处理。
  
- Composite（容器构件）：它在组合结构中表示容器节点对象，容器节点包含子节点，其子节点可以是叶子节点，也可以是容器节点，它提供一个集合用于存储子节点，实现了在抽象构件中定义的行为，包括那些访问及管理子构件的方法，在其业务方法中可以递归调用其子节点的业务方法。

## 4.C语言实现

首先定义一个抽象结构体 Component，它表示树形结构中的每一个节点，它有两个指针，一个是指向父节点，另一个是指向子节点的链表:

```c
typedef struct Component Component;
struct Component{
    void (*Add)(Component*, Component*);
    void (*Remove)(Component*, Component*);
    Component* (*GetChild)(Component*, int);
    void (*Operation)(Component*);
    Component* parent;
    Component* child_head;
};
```

然后是 Leaf 和 Composite, Leaf是叶子节点, Composite是非叶子节点

```c
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
```

接下来是对应方法的实现：

```c
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
```

在这个示例中，我们定义了一个抽象结构体 Component，它表示树形结构中的每一个节点。然后我们定义了两个具体类型， Leaf 和 Composite， Leaf是叶子节点, Composite是非叶子节点，我们通过抽象类的接口来维护组合关系 使用时, 首先创建根节点，然后在根节点下面添加叶子节点和非叶子节点, 然后调用 Operation 方法来遍历树形结构

```c
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

输出：

```c
Composite root operation
Leaf operation
Composite comp1 operation
Leaf operation
```

这样就可以通过组合模式实现树形结构的遍历了，同时可以通过 Add, Remove, GetChild 来维护树
