---
layout: post
title:  Learning Notes - Swift类型及对应Cocoa类型转换 
---

## CGFloat类型与Double类型的关系及运算

- CGFloat类型在Swift中是struct。

```
public struct CGFloat {
    /// The native type used to store the CGFloat, which is Float on
    /// 32-bit architectures and Double on 64-bit architectures.
    public typealias NativeType = Double
    public init()
    public init(_ value: Float)
    public init(_ value: Double)
    /// The native value.
    public var native: NativeType
}
```
- CGFloat对应Swift的**本地类型**是Double
- CGFloat运算时当然使用本地类型进行计算最直接
- CGFloat本身包含了一些计算方法


