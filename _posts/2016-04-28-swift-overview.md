---
layout: post
title: Swift概要（Swift Overview）
---

# Swift Overview
---
> Swift是一门新语言.Swift在设计时结合了C和ObjectiveC的许多特性，但又不受C的各种限制，Swift采用了安全的编程模型并且添加了许多新特性。

<!-- -->
> Swift采用了ObjectiveC的命名参数以及动态对象模型，从而可以无缝对接到现有的Cocoa框架。

<!-- -->
> Swift使用自动引用计数（Automatic Reference Counting, ARC）来简化内存管理。

<!--开头 -->
通常来说，编程语言教程中的第一个程序应该在屏幕上打印“Hello, world”。在 Swift 中，可以用一行代码实现：

>print("Hello, world!")

在 Swift 中，这行代码就是一个完整的程序。你不需要为了输入输出或者字符串处理导入一个单独的库。全局作用域中的代码会被自动当做程序的入口点，所以你也不需要main()函数。

你同样不需要在每个语句结尾写上分号。


## 变量（Variable）声明

变量声明语法：
>var 你的变量名称:数据类型=变量初始值

变量要符合下面的原则：

- **你的变量名称**：你可以用任何字符命名变量，比如age和🐶🐱
- **变量类型**：变量类型，比如Int和String,更多类型会在后面列出
- **变量初始值**：你要赋给变量的初始值，根据类型。


## 数据类型（Types）

Swift提供下面的基础数据类型

- **Int**：整数
- **Double**：十进位数
- **Boolean**：true,false
- **String**：string (letters and words)


## 常量 （Constants）

特定类型的变量称为常量，常量是指你只需要决定一次，但是可以使用很多次的特殊变量。

使用关键字**let**来声明常量。

```swift
let life: Int = 42
let pi: Double = 3.14
let canTouchThis: Bool = false
let captain: String = "Kirk"
```

>如果尝试给常量赋值，会得到编译器错误。


## 推断类型（Inferred Typing）

Swift提供一种新特性称之为**推断类型（Inferred Typing）**。这意味着如果在定义和初始化变量时**提供足够的信息**，Swift可以自动预测数据类型，从而不需要每次都输入类型。
在这个特性的支持下，之前的变量声明语法可以变为：

```swift
var 你的变量名称=变量初始值
let 你的常量名称=常量初始值
```

>注意描述推断类型的“**提供足够的信息**”，如果 “var intNumber=7"没有问题，但是”var doubleNumber=7“则可能达不到你的类型预期，doubleNumber会被默认推断为Int类型，提供足够的信息则需要”var doubleNumber=7.0“。

