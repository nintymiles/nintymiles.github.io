---
layout: post
title: Swift3对Foundation类的重构
---
## 如何使用`String`加载html内容

```
    let htmlStr = try! String.init(contentsOf: url!)
    
    //注意Swift3中对一些系统枚举常量的重新命名  NSStringEncoding 现在变为 String.Encoding.xxx
    let htmlString = try? String.init(contentsOf: url!, encoding: String.Encoding.utf8)  
```

`String` 是 `NSString`的替代者。 

> 注：使用`String`初始化获取url内容时，目前已知的`String.Encoding`（数目有限）往往导致获取的String为乱码，不指定Encoding时反而可以正确读取。

## 现成休眠功能调用
`sleep()`为`Thread`类的类型方法，调用如下：

```
//休眠3秒钟
Thread.sleep(forTimeInterval: 3)
```

## `Int` 和 `String` 等在 `Swift` 中是 `struct` 类型
下为Int的接口文件。

```
public struct Int : SignedInteger, Comparable, Equatable {

    /// Create an instance initialized to zero.
    public init()

    /// Creates an integer from its big-endian representation, changing the
    /// byte order if necessary.
    public init(bigEndian value: Int)

    /// Creates an integer from its little-endian representation, changing the
    /// byte order if necessary.
    public init(littleEndian value: Int)

    /// Create an instance initialized to `value`.
    public init(integerLiteral value: Int)

    /// Returns the big-endian representation of the integer, changing the
    /// byte order if necessary.
    public var bigEndian: Int { get }

    /// Returns the little-endian representation of the integer, changing the
    /// byte order if necessary.
    public var littleEndian: Int { get }

    /// Returns the current integer with the byte order swapped.
    public var byteSwapped: Int { get }

    public static var max: Int { get }

    public static var min: Int { get }
}
```

