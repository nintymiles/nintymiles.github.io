---
layout: post
title:  Learning Notes - Swift中的类型转换及与Cocoa类型的关系 
---

## Swift 的 unichar 类型 和 Character 类型 之间如何转化
unichar to Character 类型转化要经过3步

- unichar is an alias of UInt16 (typealias unichar = UInt16).
- UnicodeScalar has init(_ v: UInt16).
- Character (element of String) has init(_ scalar: UnicodeScalar).   

```
var str = "#ffffff"
var unichar = (str as NSString).characterAtIndex(0)
var unicharString = Character(UnicodeScalar(unichar))
var containsHash = unicharString == "#"                                                                                                                                                                                                                                                                                                                                                                                                                                                              
```
	
	
## Swift 中如何将 CGPoint 转化为 NSValue 类型
在Swift中

```
let point = CGPointMake(CGFloat(1.0),CGFloat(1.0))
var pointObj = NSValue(CGPoint: point)
```
> 注：Swift的Float类型和CGFloat类型需要通过CGFloat的构造函数来转化


在Objective-C中

```
CGPoint point = CGPointMake(1, 1);
 NSValue *pointObj = [NSValue valueWithCGPoint:point];
```



