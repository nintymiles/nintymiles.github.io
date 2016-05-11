---
layout: post
title: Swift概要2（Swift Overview 2）
---

# Swift Overview 2
---
>Swift can be thought of as Objective-C reimagined using modern concepts and safe programming patterns. In Apple's own words, Swift is like Objective-C without
the C. Chris Lattner, the creator of Swift, said Swift took language ideas from Objective-C, Rust, Haskell, Ruby, Python, C#, CLU, and far too many others to list. At WWDC 2014, Apple really stressed that Swift was safe by default. Swift was designed to eliminate many common programming errors, making applications more secure and less prone to bugs. Swift 2 added two additional core features to the language—availability and error handling—which are designed to make it even easier to write safe code.

## 比较操作符 （Comparison Operators)

Swift中的比较操作符用于执行简单的数学操作，比如比较数字和值。常用的操作符有：

- **>**     大于（Greater Than）
- **<**     小于（Less Than）
- **==**    等于（Equal To）
- **>=**    大于等于（Greater Than or Equal To ）
- **&&**    和（AND）
- **\|\|**    或（OR）



## 字符插入 （String Interpolation）

将String输出到控制台意义重大^_^,如何在print语句中将变量组合为String或者合适的值将更有价值。在Swift中，这种操作称为String Interpolation。

```swift
var apples = 5
print("Sally has \(apples) apples")
```

看到上面的例子吗？使用格式：**\\(variable name)**，就可以将任意变量的值转换为String。是不是很容易。在括号中甚至可以进行数学操作，如下：

```swift
print("Sally has \(apples - 5) apples")
```

## If/else语句 (If/else statements)

```swift
if (batmanCoolness > spidermanCoolness) {
  spidermanCoolness = spidermanCoolness - 1
} else if (batmanCoolness >= spidermanCoolness) {
  spidermanCoolness = spidermanCoolness - 1
} else {
  spidermanCoolness = spidermanCoolness + 1
}
```

## While循环 （While Loops）

```swift
var secondsLeft = 3
while (secondsLeft > 0) {
  print(secondsLeft)
  secondsLeft = secondsLeft - 1
}
print("Blast off!")
```

## Break和Continue语句

```swift
var cokesLeft = 7
var fantasLeft = 4
while(cokesLeft > 0)  {
  print("You have \(cokesLeft) Cokes left.")
  cokesLeft = cokesLeft - 1
  if(cokesLeft <= fantasLeft)  {
break }
}
print("You stop drinking Cokes.")
```

```swift
var numbers = 0
while(numbers <= 10)  {
  if(numbers == 9)  {
    numbers = numbers + 1
    continue
  }
  print(numbers)
  numbers = numbers + 1
}
```

