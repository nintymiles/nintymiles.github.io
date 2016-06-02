---
layout: post
title: Swift Notes - Classes and Structures
---

##  Classes and Structures
Definition:Classes and structures are general-purpose, flexible constructs that become the building blocks of your program’s code.

- same features
	There are some common features in swift  between classes and structures:
	1. Define properties to store values  - 定义属性以存值
	2. Define methods to provide functionality - 定义方法以提供功能
	3. Define subscripts to provide access to their values by using subscript syntax - 定义下标以提供使用下标语法进行值的访问
	4. Define initializers to set up their initial state - 定义初始化器以设置初始状态
	5. Be extended to expand their functionality beyond a default implementation. - 可以被延伸扩展功能，而不是基于默认实现
	6. Conform to protocols to provide standard functionality of a certain kind - 符合协议以提供某种标准功能

- different features
	Classes own features exclusively:
	1.  Inheritance enables one class to have characteristics of another - 类可以继承拥有其他类的特征，而structure不能
	2. Type casting enables you to check and interpret the types of a class instance at runtime - 类型转换可以在运行时检查和解析类实例的类型，而structure不能
	3. Deinitializers enable an instance of a class to free up any resources it has assigned - 解构器可以让类实例释放已分配的任何资源
	4. Reference counting allows more than one reference to a class instance - 引用计数机制允许类实例可以拥有超过一个（**多个**）引用存在

> Structures are always copied when they are passed around in your code, and do not use reference counting.

**Structures are *value* types and Classes are *reference* types.**

## Definition Syntax 
Classes and structures have a **similar definition syntax**. You introduce classes with the **class** keyword and structures with the **struct** keyword. 

```
class SomeClass {
// class definition goes here`
}

struct SomeStructure {
// structure definition goes here
}
```

## Identity Operators (身份操作符)
We can use identity operators to check whether two constants or variables refer to the same single instance. There are two kinds of identity operators:
1. Identical to (**===**) 
2. Not identical to (**!==**)


```
if tenEighty === alsoTenEighty {
    print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
}
// Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance.”

```

> “Note that “identical to” (represented by ***three equals signs***, or ===) does not mean the same thing as “equal to” (represented by two equals signs, or ==)”

### two concepts
- “Identical to” means that two constants or variables of class type refer to exactly the same class instance.
- “Equal to” means that two instances are considered “equal” or “equivalent” in value, for some appropriate meaning of “equal”, as defined by the type's designer.
