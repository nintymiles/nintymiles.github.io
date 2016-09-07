---
layout: post
title:  Learning Notes - Swift Optional Variables review
---
## Swfit Optional Variables review
In Swift, an optional variable is a variable that we *are able to* assign **nil** (no value) to . So (no optional) variables means the variables are required to have a **non-nil** value.

### how to understand swift imports optional variables as a ***safety*** feature？
Optional variables were added to the Swift language as a safety feature. They provide a **compile time check** of our variables to verify that they contain a **valid** value. *Unless our code specically defines a variable as optional*, we can assume that the variable contains a valid value, and we do not have to check for nil values. 

### The ways to verify optional?
There are three ways to verify(avoid verifying) optional containing value:

1. First thought may be to use the ***!= (not equals to) operator*** to verify that the variable is not equal to nil, 
2. **Optional Binding**
3. **Optional Chaining**(？理解为optional通过链式传递，传递过程中按照规则免验证)

### How to use an optional's value?
To use **force unwrapping**(强制解绑就是为了使用optional的值而设计), we must *first make sure that the optional has a non-nil value* and then we can use the **explanation point** to **access** that value. The following example shows how we can do this:   
```
   var name: String?   Name = "Jon"   if name != nil {       var newString = "Hello " + name!  }
```

### The conventions to use optional binding
We use optional binding to check whether an optional variable or constant has a non-nil value, and, if so, assign that value to a temporary variable. For optional binding, we use the if-let or if-var keywords together. If we use if-let, the temporary value is a constant and cannot be changed, while the if-var keywords puts the temporary value into a variable that allows us to change the value. 

```
if let temp = myOptional {       print(temp)       print("Can not use temp outside of the if bracket")   } else {       print("myOptional was nil")   }
```

It is **perfectly acceptable** with optional binding to *assign the value to a variable of the same name*. The following code illustrates this:(此处用法异于其它语言，但是从swift本身的语法是合法的。**特异之处**。)

```   if let myOptional = myOptional {       print(myOptional)       print("Can not use temp outside of the if bracket")   } else {       print("myOptional was nil") }
```

We can also test **multiple optional variables in one line**（支持一行多个optional变量，以逗号隔开）. We do this by separating each optional check with a ***comma***. The following example shows how to do this:   
```
   if let myOptional = myOptional, myOptional2 = myOptional2,   myOptional3 = myOptional3 {     // only reach this if all three optionals     // have non-nil values   }
```


