## About GeneratorType

```
/// Encapsulates iteration state and interface for iteration over a
/// sequence.
///
/// - Note: While it is safe to copy a generator, advancing one
///   copy may invalidate the others.
///
/// Any code that uses multiple generators (or `for`...`in` loops)
/// over a single sequence should have static knowledge that the
/// specific sequence is multi-pass, either because its concrete
/// type is known or because it is constrained to `CollectionType`.
/// Also, the generators must be obtained by distinct calls to the
/// sequence's `generate()` method, rather than by copying.
```

```
public protocol GeneratorType {
    /// The type of element generated by `self`.
    associatedtype Element
    /// Advance to the next element and return it, or `nil` if no next
    /// element exists.
    ///
    /// - Requires: `next()` has not been applied to a copy of `self`
    ///   since the copy was made, and no preceding call to `self.next()`
    ///   has returned `nil`.  Specific implementations of this protocol
    ///   are encouraged to respond to violations of this requirement by
    ///   calling `preconditionFailure("...")`.
    @warn_unused_result
    public mutating func next() -> Self.Element?
}
```

## Nonescaping Closures
A closure is said to escape a function when the closure is passed as an argument to the function, but is called after the function returns. When you declare a function that takes a closure as one of its parameters, you can write @noescape before the parameter name to indicate that the closure is not allowed to escape. Marking a closure with @noescape lets the compiler make more aggressive optimizations because it knows more information about the closure’s lifespan.

```
func someFunctionWithNonescapingClosure(@noescape closure: () -> Void) {
    closure()
}
```
As an example, the sort(_:) method takes a closure as its parameter, which is used to compare elements. The parameter is marked @noescape because it is guaranteed not to be needed after sorting is complete.

One way that a closure can escape is by being stored in a variable that is defined outside the function. As an example, many functions that start an asynchronous operation take a closure argument as a completion handler. The function returns after it starts the operation, but the closure isn’t called until the operation is completed—the closure needs to escape, to be called later. For example:

```
var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHandler: () -> Void) {
    completionHandlers.append(completionHandler)
}
```

The someFunctionWithEscapingClosure(_:) function takes a closure as its argument and adds it to an array that’s declared outside the function. If you tried to mark the parameter of this function with @noescape, you would get a compiler error.

Marking a closure with @noescape lets you refer to self implicitly within the closure.

```
class SomeClass {
    var x = 10
    func doSomething() {
        someFunctionWithEscapingClosure { self.x = 100 }
        someFunctionWithNonescapingClosure { x = 200 }
    }
}
 
let instance = SomeClass()
instance.doSomething()
print(instance.x)
// Prints "200"
 
completionHandlers.first?()
print(instance.x)
// Prints "100"
```


