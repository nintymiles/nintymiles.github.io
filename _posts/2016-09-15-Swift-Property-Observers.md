---
layout: post
title:  Learning Notes - Swift Property observers 、type property、global／Local variables
---
## Swift Property 的监控机制 Property Observers
You have the option to define either or both of these observers on a property:

- willSet is called just before the value is stored.
- didSet is called immediately after the new value is stored. 

Property observers 一般用在stored properties，nooverride computed proerties由于本身实现代码块中即可包含监控代码，故一般不需要考虑property observers.

### `willSet`和`didSet`都包含一个默认constant parameter用来传递新的或者旧的属性值，parameter name可以指定，也可以不指定由系统生成默认值`newValue`或者`oldValue`（不指定参数，有系统隐含生成）
If you implement a willSet observer, it’s passed the new property value as a constant parameter. You can specify a name for this parameter as part of your willSet implementation. If you don’t write the parameter name and parentheses within your implementation, the parameter is made available with a default parameter name of newValue.

Similarly, if you implement a didSet observer, it’s passed a constant parameter containing the old property value. You can name the parameter or use the default parameter name of oldValue. If you assign a value to a property within its own didSet observer, the new value that you assign replaces the one that was just set.  

## Global/Local variables
**Global variables** are variables that are defined outside of any function, method, closure, or type context. 

**Local variables** are variables that are defined within a function, method, or closure context.
 
However, you can also define computed variables and define **observers** for stored variables, ***in either a global or local scope***. Computed variables calculate their value, rather than storing it, and they are written in the same way as computed properties.

## Type Property
You can also define properties that belong to the type itself, not to any one instance of that type. There will only ever be one copy of these properties, no matter how many instances of that type you create. These kinds of properties are called type properties.

Type properties are useful for defining values that are universal to all instances of a particular type, such as a constant property that all instances can use (like a static constant in C), or a variable property that stores a value that is global to all instances of that type (like a static variable in C).

### Type Property Syntax
In C and Objective-C, you define static constants and variables associated with a type as global static variables. In Swift, however, type properties are written as part of the type’s definition, within the type’s outer curly braces, and each type property is explicitly scoped to the type it supports.

You define type properties with the **static** keyword. For ***computed type properties*** for ***class*** types, you can use the ***class*** keyword instead to allow subclasses to override the superclass’s implementation. 

```
struct SomeStructure {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 1
    }
}
```

```
class SomeClass {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 27
    }
    class var overrideableComputedTypeProperty: Int {
        return 107
    }
}
```


