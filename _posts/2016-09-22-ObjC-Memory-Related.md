---
layout: post
title:  Learning Notes - ObjC Memory Related
---
##Retain cycles (引用循环问题)
A retain cycle occurs when two objects such as a parent and child object have strong references to each other. A simple example would be the following code:   
```
	@interface MyParent : NSObject   @property (strong) MyChild *myChild;   @end   @interface MyChild : NSObject   @property (strong) MyParent *myParent;   @end
```You can create an object of the type MyParent with the following code: 

`MyParent *myParent = [[MyParent alloc] init];` 

In the above code, you can quickly see what is called a retain cycle since myParent has a strong reference to myChild, and myChild has a strong referenceto myParent. This is a form of memory leak where if an object tries to release an instance of the  rst object, it can't be released because the second object has a strong reference to the  rst object and a retain cycle is created. **Do note** that **ARC** will not fix all memory leaks for you

A **general rule** of thumb to avoid a situation where a retain cycle can occur is to remember this—if object A wants to retain object B indefinitely, then object A hasto be higher up in the hierarchy tree than object B, where object A has to have a strong reference to object B. If you have objects that are **on the same level** in the hierarchy tree, then you **should put a weak reference** to avoid a retain cycle. So inthe preceding code, to avoid a retain cycles, mySecondObject should not have a strong reference to myFirstObject. However, if you do need to let mySecondObject have a reference to myFirstObject, then make it a weak reference instead of a strong reference. Tree hierarchies are safe and do remember that putting weak references will avoid a retain cycle and memory leaks.

