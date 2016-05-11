---
layout: post
title: Swift Learning Notes part 2
---

# Swift Learning Notes part 2
---
>Swift can be thought of as Objective-C reimagined using modern concepts and safe programming patterns. In Apple's own words, Swift is like Objective-C without
the C. Chris Lattner, the creator of Swift, said Swift took language ideas from Objective-C, Rust, Haskell, Ruby, Python, C#, CLU, and far too many others to list. At WWDC 2014, Apple really stressed that Swift was safe by default. Swift was designed to eliminate many common programming errors, making applications more secure and less prone to bugs. Swift 2 added two additional core features to the language—availability and error handling—which are designed to make it even easier to write safe code.

## Optional variables
In Swift,normally,the variables are considered to be nonoptional. an optional variable is a variable that we are able to assign nil (no value) to. **Optional variables and constants are defined using *?* (question mark).** 

>Optional variables were added to the Swift language as a safety feature. 

To use force unwrapping, we must  rst make sure that the optional has a non-nil value and then we can use the explanation point to access that value. 

```
var name: String?
Name = "Jon"
if name != nil {
    var newString = "Hello " + name!
}
```

We use **optional binding** to check whether an optional variable or constant has a non-nil value, and, if so, assign that value to a temporary variable. For optional binding, we use the **if-let** or **if-var** keywords together. If we use if-let, the ***temporary*** value is a constant and cannot be changed, while the if-var keywords puts the temporary value into a variable that allows us to change the value. 


```
if let temp = myOptional {
    print(temp)
    print("Can not use temp outside of the if bracket")
} else {
    print("myOptional was nil")
}
```

We can also test multiple optional variables in one line. We do this by separating each optional check with a comma. The following example shows how to do this:
```
   if let myOptional = myOptional, myOptional2 = myOptional2,
   myOptional3 = myOptional3 {
     // only reach this if all three optionals
     // have non-nil values
   }
```
>Within the optional binding zone,variable's changing is independent.so it doesn't affect the value of the outter variable,even the variable is declared outter the scope.

**Optional chaining** allows us to call properties, methods, and subscripts on an optional that might be nil. If any of the chained values return nil, the return value will be nil. 

```
var s = car?.tires?.tireSize
```