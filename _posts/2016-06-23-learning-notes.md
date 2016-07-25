---
layout: post
title:  Learning Notes - Swift中关于tuple的赋值形式
---

## tuple中变量的赋值过程

### 赋值表达式(Assignment Operator)
赋值表达式(=)会对某个给定的表达式赋值。 它有如下的形式；

expression = value

赋值操作符（＝）把右边的 **value** 赋值给左边的 **expression**. 如果左边的expression 需要接收多个参数(比如是一个tuple)，那么右边必须也是一个具有同样数量参数的tuple. (***允许嵌套的tuple***)


```
(a, _, (b, c)) = ("test", 9.45, (12, 3))
// a is "test", b is 12, c is 3, and 9.45 is ignored
```
> tuple的这种赋值形式对于批量赋值来说很简洁有效，但是并不直观容易理解。

**赋值运算符不返回任何值**。

### Assignment Operator
The assignment operator (a = b) initializes or updates the value of a with the value of b:


```
let b = 10
var a = 5
a = b
// a is now equal to 10
```

If the right side of the assignment is a tuple with multiple values, its elements can be decomposed into multiple constants or variables at once:

```
let (x, y) = (1, 2)
// x is equal to 1, and y is equal to 2
```

Unlike the assignment operator in C and Objective-C, **the assignment operator in Swift *does not itself return a value***. The following statement is not valid:


```
if x = y {
    // this is not valid, because x = y does not return a value
}
```


