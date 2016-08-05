---
layout: post
title:  Learning Notes - About Swift's default parameter values
---

## Swift函数可以拥有默认参数值
当我们在参数（类型）后定义了默认值后，在调用函数的时候可以选择忽略这个参数调用。如果一个函数有两个参数，并且都设置了默认值，那么函数调用会有以下多种形式。

```
public func toPinyin(withFormat outputFormat: PinyinOutputFormat = PinyinOutputFormat.defaultFormat, separator: String = " ") -> String{
...
}

hanziString.toPinyin()  //无参
hanziString.toPinyin(withFormat: PinyinOutputFormat.defaultFormat) //前参
hanziString.toPinyin(separator:"_") //后参
hanziString.toPinyin(withFormat: PinyinOutputFormat.defaultFormat，separator:"_") //两参数
```
> 这种函数调用的自由度意味着多变的代码风格,对swift语言不熟悉的程序员将更加难以读懂代码。


## Default Parameter Values
You can define **a default value for any parameter** in a function by assigning a value to the parameter after that parameter’s type. If a default value is defined, you can ***omit*** that parameter when calling the function.

```
func someFunction(parameterWithDefault: Int = 12) {
    // function body goes here
    // if no arguments are passed to the function call,
    // value of parameterWithDefault is 12
}
someFunction(6) // parameterWithDefault is 6
someFunction() // parameterWithDefault is 12
```

>Place parameters with default values at the end of a function’s parameter list. This ensures that all calls to the function use the same order for their nondefault arguments, and makes it clear that the same function is being called in each case.




