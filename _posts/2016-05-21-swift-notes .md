---
layout: post
title: Swift Learning Notes  - Closure
---

## Closure Definition
*Closures are **self-contained blocks of functionality** that can be **passed around** and **used** in your code.*

- Closures are similar to blocks in C and Objective C
- Closures can **capture** and **store** **references** to ***any*** constants and variables *from the context in which they are defined*.
- Global and nested functions are special cases of closures,not all kinds of functions.
	- Global functions are closures that have a name and do not capture any values
	- Nested functions are closures that have a name and can capture values from their enclosing function
	- Closure expressions are unnamed closures written in a lightweight syntax that can capture values from their surrounding context.
- **Closure syntax optimisation**
	- Inferring parameter and return value types from context Implicit returns from single-expression closures (*two key point here*)
	- Shorthand argument name
	- Trailing closure syntax
- **Closure syntax expression**

```

{(parameters)  ->  return type in      
    code      //code break in a new line is necessary for this sort of expression
}
```

Code implementation block must be started on a new line,even in optimization style。otherwise it will raise error in playground.
	
```
let reversed=names.sort({(s1: String,s2: String) -> Bool in
        return s1 > s2 })

let revesed2=names.sort({(s1,s2) in
        return s1 > s2});

let reversed3=names.sort({s1,s2 in
        s1 > s2})
        
let reversed4=names.sort({
        $0 < $1})
        
let reversed5=names.sort( < ) //The most simpliest way is operator function. no new line required, but it seems weired.
```
- Operator function，**Swift’s String type defines its string-specific implementation of the greater-than operator (>) as a function that has two parameters of type String, and returns a value of type Bool*.** *This exactly matches the function type needed by the sort(_:) method. *So the operator function simple style is just a special case? *
- Trailing closure,**A trailing closure is a closure expression that is written outside of (and after) the parentheses of the function call.**  If you need to pass a closure expression to a function as the function’s final argument and the closure expression is long, it can be useful to write it as a trailing closure instead.
- Trailing closure more simpler style,If a closure expression is provided as the function or method’s only argument and you provide that expression as a trailing closure, *you do not need to write a pair of parentheses ()* after the function or method’s name when you call the function

	
> Collection classes’ sort(\_:) method_will produce a new collection that has the same size and type as the old one. the original collection is not modified by the sort(\_:) method._

## Function Parameter Names
Function has two kind of parameter name,one is external parameter name,the other is local parameter name.
- external parameter name is used to label arguments passed to a function call
- local parameter name  is used in the implementation of the function