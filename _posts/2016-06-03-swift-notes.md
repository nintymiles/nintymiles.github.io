---
layout: post
title: Swfit Notes - Lazy关键字
---

## Lazy stored properties
A **lazy** stored property is a property whose initial value is not calculated **until the first time it is used**. You indicate a lazy stored property *by writing the **lazy modifier** before its declaration*.

> lazy关键字标识的属性的含义是，这个属性的值将在第一次使用前才会被初始化，虽然可能已经填写了初始化代码。

> 另注：lazy 属性只能声明为变量，常量无效报错。

Lazy properties are useful when the initial value for a property is dependent on **outside factors** whose values are not known until after an *instance’s initialization is complete*. 