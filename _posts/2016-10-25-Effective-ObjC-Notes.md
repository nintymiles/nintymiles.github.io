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

##Literal Syntax
- Literal->Subscripting,Subscripting支持NSArray,NSDictionary

`NSString *dog=animals[1];`

- The literal syntax works for **expressions**.

```
int x=5;
float y=6.32f;
NSNumber *expressionNumber=@(x*y);
```

- 如何使用Literal生成一个Mutable变量

`NSMutableArray *mutableArr=[@[@1,@2,@3] mutableCopy];`

###Literal的重点
- 使用Literal Syntax生成Strings，numbers，arrays，和Dictionaries。代码整洁，简明
- 通过下标subscripting方法访问array的索引或者dictionary的key
- 当尝试插入nil时，literal syntax会抛出异常，因而避免其中的值为nil

> `succinct（简洁的，简明的）`                                                       

