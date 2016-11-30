---
layout: post
title: Objective C Compiler error caused by global object variable initialization
---
## Compiler error: “initializer element is not a compile-time constant”
When you define a variable outside the scope of a function, that variable's value is actually written into your executable file. **This means you can only use a constant value**. Since you don't know everything about the runtime environment at compile time (which classes are available, what is their structure, etc.), **you cannot create objective c objects until runtime**, with the exception of constant strings, which are given a specific structure and guaranteed to stay that way. What you should do is initialize the variable to nil and use +initialize to create your image. initialize is a class method which will be called before any other method is called on your class.

Example:

```
NSImage *imageSegment = nil;
+ (void)initialize {
    if(!imageSegment)
        imageSegment = [[NSImage alloc] initWithContentsOfFile:@"/User/asd.jpg"];
}
- (id)init {
    self = [super init];
    if (self) {
        // Initialization code here.
    }

    return self;
} 
```

