---
layout: post
title: Learning Notes - Swift Operators
---

## Nil Coalescing operator (??)
The **nil coalescing operator** (*a ?? b*) unwraps an optional a if it contains a value, or returns a default value b if a is nil. The expression a is always of an optional type. *The expression b must **match** the type that is stored inside a.*

**So it is a special tenary operator case for optional value operating.**

The nil coalescing operator is shorthand for the code below:


```
a != nil ? a! : b
```

## Range Operators
Swift includes two range operators, which are shortcuts for expressing a range of values.
- **Closed Range Operator**     (闭合范围操作符)

    The closed range operator (a...b) defines a range that runs from a to b, and includes the values a and b. The value of a must not be greater than b.

```
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```


- **Half Open Range Operator**  (半开范围操作符)

    The half-open range operator (**a..<b**) defines a range that runs from a to b, but does not include b. It is said to be half-open because it contains its first value, but not its final value. As with the closed range operator, the value of a must not be greater than b. If the value of a is equal to b, then the resulting range will be empty.
    

```
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
    print("Person \(i + 1) is called \(names[i])")
}
// Person 1 is called Anna
// Person 2 is called Alex
// Person 3 is called Brian
// Person 4 is called Jack
```

> Half-open ranges are particularly useful when you work with zero-based lists such as arrays


