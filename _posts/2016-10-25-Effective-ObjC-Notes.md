---
layout: post
title:  Learning Notes - Objective-C 
---
##Why using **forward declaring the class**  
Deferring the import to where it is required enables you to limit the **scope** of **what** a consumer of your class needs to **import**. 任何在a头文件中引入的头文件b，在a头文件被别的类引入时,也会同时引入所有的b头文件。如果引入链条继续，最终当然会增加编译时间。结论，在头文件中尽量不引入其他头文件，使用**Forward Declaring**方式实现延迟**importing**。

**Forward Declaring**也可以减轻类互相引用，导致retain cycle的问题。

总结:
1. 头文件中优先**Forward Declared**
2. Property,Protocol优先写在category中


```
@class EOCEmployer

@interface: EOCPerson

...
@property(strong) EOCEmployer *employer;
...

@end
```

> *class-continuation category*  `alleviate(减轻，缓和）`  `chicken-and-egg(先有鸡还是先有蛋风格的）` intricacy()  paradigm()

