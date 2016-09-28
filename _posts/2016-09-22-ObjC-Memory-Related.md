---
layout: post
title:  Learning Notes - ObjC Memory Related
---
##Retain cycles (引用循环问题)

```
	@interface MyParent : NSObject
```

`MyParent *myParent = [[MyParent alloc] init];` 

In the above code, you can quickly see what is called a retain cycle since myParent has a strong reference to myChild, and myChild has a strong reference

A **general rule** of thumb to avoid a situation where a retain cycle can occur is to remember this—if object A wants to retain object B indefinitely, then object A has
