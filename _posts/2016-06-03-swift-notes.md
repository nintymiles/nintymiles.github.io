---
layout: post
title: Swfit Notes - Lazy关键字
---

## Lazy stored properties
A **lazy** stored property is a property whose initial value is not calculated **until the first time it is used**. You indicate a lazy stored property *by writing the **lazy modifier** before its declaration*.

> lazy关键字标识的属性的含义是，这个属性的值将在第一次使用前才会被初始化，虽然可能已经填写了初始化代码。

> 另注：lazy 属性只能声明为变量，常量无效报错。

Lazy properties are useful when the initial value for a property is dependent on **outside factors** whose values are not known until after an *instance’s initialization is complete*. 

## Swift’s Properties
Properties associate values with a particular *class*, *structure*, or *enumeration*. There are two kind of properties:
- stored properties, Stored properties store constant and variable values as part of an instance, Stored properties are provided only by classes and structures.
- computed properties,whereas computed properties calculate (rather than store) a value. Computed properties are provided by classes, structures, and ***enumerations***.


## How to access properties
You can access the properties of an instance using **dot syntax**. In dot syntax, you write the property name immediately after the instance name, separated by a period (.), **without any spaces**.

## Reference Types
Effectively,reference types are just the different names for the same instance.

## Choosing Between Classes and Structures
Both classes and structures can be use as the build blocks of programmer’s code.

Normally, structure instances are always passed by value, and class instances are always passed by reference. This means that they are suited to different kinds of tasks.

A general guideline for using structures:
- The structure’s primary purpose is to encapsulate a few relatively simple data values.
- It is reasonable to expect that the encapsulated values will be copied rather than referenced when you assign or pass around an instance of that structure.
- Any properties stored by the structure are themselves value types, which would also be expected to be copied rather than referenced.
- The structure does not need to inherit properties or behavior from another existing type.
