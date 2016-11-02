---
layout: post
title: Swift Excerpt： Selector in Swift
---
## Selector in Swift
### Selector Expression
A selector expression lets you access the selector used to refer to a method in Objective-C.

```
 #selector(method name)
```
The method name must be a reference to a method that is available in the Objective-C runtime. The value of a selector expression is an instance of the Selector type. For example:

```
class SomeClass: NSObject {
    @objc(doSomethingWithInt:)
    func doSomething(x: Int) { }
}
let x = SomeClass()
let selector = #selector(x.doSomething(_:))
```
The method name can contain parentheses for grouping, as well the as operator to disambiguate between methods that share a name but have different type signatures. For example:

```
extension SomeClass {
    @objc(doSomethingWithString:)
    func doSomething(x: String) { }
}

let anotherSelector = #selector(x.doSomething(_:) as (String) -> Void)
```

Because the selector is created at compile time, not at runtime, the compiler can check that the method exists and that the method is exposed to the Objective-C runtime.

> Although the method name is an expression, it is never evaluated.

For more information about using selectors in Swift code that interacts with Objective-C APIs, see Objective-C Selectors in Using Swift with Cocoa and Objective-C (Swift 2.2).

`selector-expression → #selector(expression)`


