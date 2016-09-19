---
layout: post
title:  Learning Notes - Swift 3 Changes
---
## Removing var from Function Parameters

```
func doSomethingWithVar(var i: Int) {
  i = 2 // This will NOT have an effect on the caller's Int that was passed, but i can be modified locally
}

func doSomethingWithInout(inout i: Int) {
  i = 2 // This will have an effect on the caller's Int that was passed.
}

var x = 1
print(x) // 1

doSomethingWithVar(x)
print(x) // 1

doSomethingWithInout(&x)
print(x) // 2 
```

### Motivation
Using var annotations on function parameters have limited utility, optimizing for a line of code at the cost of confusion with inout, which has the semantics most people expect. To emphasize the fact these values are unique copies and don't have the write-back semantics of inout, we should not allow var here.

In summary, the problems that motivate this change are:

var is often confused with inout in function parameters.
var is often confused to make value types have reference semantics.
Function parameters are not refutable patterns like in if-, while-, guard-, for-in-, and case statements.

### Impact on exist code
As a purely mechanical migration away from these uses of var, a temporary variable can be immediately introduced that shadows the immutable copy in all of the above uses. For example:

```
func foo(i: Int) {
  var i = i
}
```

