---
layout: post
title:  Learning Notes - Designated and Convinence Initializer
---

## What is Designated and Convinence initializer?
- Designated initializers are the **primary initializers** for a class.Classes tend to have very few designated initializers, and it is quite common for a class to have only one. Designated initializers are “funnel” points through which initialization takes place, and through which the initialization process continues up the superclass chain.
- Convenience initializers are **secondary, supporting initializers** for a class.You can also define a convenience initializer to create an instance of that class for a specific use case or input value type.

## Syntax for Designated and Convenience Initializers
Designated initializers for classes are written in the same way as simple initializers for value types:

```
init(parameters) {
    statements
}
```
Convenience initializers are written in the same style, but with the convenience modifier placed before the init keyword, separated by a space:

```
convenience init(parameters) {
    statements
}
```

## Initializer Delegation for Class Types
To simplify the relationships between designated and convenience initializers, Swift applies the following three rules for delegation calls between initializers:

Rule 1
A designated initializer **must call** a designated initializer from its immediate superclass.

Rule 2
A convenience initializer **must call** another initializer from the same class.

Rule 3
A convenience initializer must ultimately call a designated initializer.

> Designated initializers must always delegate up.
> Convenience initializers must always delegate across.



