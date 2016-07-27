---
layout: post
title:  Learning Notes - Swift中的类型转换及与Cocoa类型的关系 
---

## Swift 的 unichar 类型 和 Character 类型之间如何转化
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

## Swift Charactr(字符） 变量
Swift的字符可以理解为**只有一个字符**的字符串，数据类型为 Character，如果尝试在 Character（字符） 类型中存储更多的字符，则程序会报错。

```
import Cocoa

let char1: Character = "A"
let char2: Character = "B"


print("char1 的值为 \(char1)")
print("char2 的值为 \(char2)")

// Swift 中以下赋值会报错
//let char: Character = "AB"
```

## 遍历字符串中的字符
Swift 的 String 类型表示特定序列的 Character（字符） 类型值的集合。 每一个字符值代表一个 Unicode 字符。

```
import Cocoa

for ch in "Hello".characters {
   print(ch)
}
```

## Swift 的标准字符串类型 String

字符串函数及运算符解释

1.	 **isEmpty** 判断字符串是否为空，返回布尔值
2.	 **hasPrefix**(prefix: String) 检查字符串是否拥有特定前缀
3.	 **hasSuffix**(suffix: String) 检查字符串是否拥有特定后缀。
4.	 **Int**(String) 转换字符串数字为整型。 实例:
```
let myString: String = "256"
let myInt: Int? = Int(myString)
```
5.	 **String.characters.count** 计算字符串的长度
6.	 **utf8** 您可以通过遍历 String 的 utf8 属性来访问它的 UTF-8 编码
7. 	**utf16** 您可以通过遍历 String 的 utf16 属性来访问它的 UTF-16 编码
8.	 **unicodeScalars** 您可以通过遍历String值的unicodeScalars属性来访问它的 Unicode 标量编码.
9.	 **+** 连接两个字符串，并返回一个新的字符串
10. **+=** 连接操作符两边的字符串并将新字符串赋值给左边的操作符变量
11. **==** 判断两个字符串是否相等
12. **<** 比较两个字符串，对两个字符串的字母逐一比较。
13. **!=** 比较两个字符串是否不相等。


