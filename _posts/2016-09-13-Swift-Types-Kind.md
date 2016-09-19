---
layout: post
title:  Learning Notes - Swift‘s value and reference types
---
##Swift's Value Types
A value type is a type whose value is copied when it is assigned to a variable or constant, or when it is passed to a function.

In fact, all of the basic types in Swift—integers, floating-point numbers, Booleans, **strings**, **arrays** and **dictionaries**—are value types, and *are implemented as structures behind the scenes*.

> 这么多类型都是 `Value Type`

All **structures and enumerations** are value types in Swift. This means that any structure and enumeration instances you create—and any value types they have as properties—are always copied when they are passed around in your code.

## Reference Types
Unlike value types, **reference types are *not* copied** when they are assigned to a variable or constant, or when they are passed to a function. Rather than a copy, *a reference to the same existing instance* is used instead.

## Identity Operators
In Swift,it can sometimes be useful to find out if two constants or variables refer to exactly the same instance of a class. To enable this, Swift provides two identity operators:

- **Identical to** (===)
- **Not identical to** (!==)

`Identical to` 和 `Equal to` 区别

- “**Identical** to” (===) means that two constants or variables of class type *refer to exactly the same class instance*.
- “**Equal** to” (==) means that two instances are *considered “equal” or “equivalent” in value*, for some appropriate meaning of “equal”, as defined by the type's designer.

## String as a value type
Swift’s String type is a value type. If you create a new String value, that String value is copied when it is passed to a function or method, or when it is assigned to a constant or variable. In each case, a new copy of the existing String value is created, and the new copy is passed or assigned, not the original version. Value types are described in Structures and Enumerations Are Value Types.

Swift’s copy-by-default String behavior ensures that when a function or method passes you a String value, it is clear that you own that exact String value, regardless of where it came from. You can be confident that the string you are passed will not be modified unless you modify it yourself.

Behind the scenes, Swift’s compiler optimizes string usage so that actual copying takes place only when absolutely necessary. This means you always get great performance when working with strings as value types.

> 和其它语言相比，可以理解为`swift`中`string`作为`reference type`的控制权被剥夺。


