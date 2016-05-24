---
layout: post
title: Swift Learning Notes  - Function
---

## Variadic parameters
A variadic parameter accepts zero or more values of a specified type.Write variadic parameters by inserting three period characters (...) after the parameter’s type name.

> A function may have at most one variadic parameter.

## In-Out parameters
You write an in-out parameter by placing the inout keyword at the **start** of its parameter definition. An ***in-out*** parameter has a value that is passed in to the function, is modified by the function, and is passed back out of the function to replace the original value.

> Usually, function parameters are **constants** by default. Trying to change the value of a function parameter from within the body of that function results in a compile-time error. This means that you can’t change the value of a parameter by mistake.


```
func swapTwoInts(inout a: Int, inout _ b: Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

## Function types
 function types just like any other types in Swift.
 
```
var mathFunction: (Int, Int) -> Int = addTwoInts
```

- Function types as parameter types
    You can use a function type such as (Int, Int) -> Int as a parameter type for another function. This enables you to leave some aspects of a function’s implementation for the function’s caller to provide when the function is called.


```
func printMathResult(mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}
printMathResult(addTwoInts, 3, 5)
// Prints "Result: 8"
```

- Function types as return types
    You can use a function type as the return type of another function. You do this by writing a complete function type immediately after the return arrow (->) of the returning function.

## Nested functions
All of the functions you have encountered so far in this chapter have been examples of **global** functions, which are defined at a **global scope**. You can also define functions **inside the bodies** of other functions, known as nested functions.

> Nested functions are hidden from the outside world by default, but can still be called and used by their enclosing function. An enclosing function can also **return** one of its nested functions to allow the nested function to be used in **another scope**.

> **Nested functions can make implementation certain level privacy**


```
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backwards ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```
