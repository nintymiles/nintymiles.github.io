---
layout: post
title:  Learning Notes - Effective Objective-C 
---
## Why using **forward declaring the class**  
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

## Literal Syntax
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

### Literal的重点
- 使用Literal Syntax生成Strings，numbers，arrays，和Dictionaries。代码整洁，简明
- 通过下标subscripting方法访问array的索引或者dictionary的key
- 当尝试插入nil时，literal syntax会抛出异常，因而避免其中的值为nil

> `succinct（简洁的，简明的）`                                                       
## Prefer typed constants to Preprocessor #define 类型化的常量优于预处理器宏定义
Normally you take the approach of defining the constant like this（采用xxx这样的方式）:
`#define Animation_Duration 0.3` 
This is a preprocessor directive,whenever the string Animation_Duration is found in your source code,it is replaced with 0.3.  （看到预处理指令，就进行替换）。此种方式缺点时没有类型信息。  **任何引入有预处理器指令头文件**的地方，都会看到替换模式。

There is always a better way to define a constant than using a preprocessor define.

`static const NSTimeInterval kAnimationDuration = 0.3;`

In fact,when declaring the variable as both **satic** and **const**,the compiler *doesn't end up creating a symbol* at all but instead replaces occurrentces just **like a preprocessor define does**. Remember,however,the benefit is that the *type information is present*.  类型信息

### How to expose a constant externally?
Such constants need to appear in the global symbol table to be used outside the translation unit in which they are defined. 这种情形下常量需要以不同于`static const`的方式声明。

```
//In the header file
extern NSString *const stringConst;

//In the implementation file
NSString *const stringConst=@"Value";
```

`extern`关键字仅只告诉编译器常量可以在看不到定义的情况下使用，编译器知道常量在binary被连接时会存在。这个常量必须被定义一次而且也仅能一次。

> A preprocessor define could be redefined by mistake,meaning that different parts of an application end up using different values. 预处理器定义可以随时被错误的重新定义，而没有类型和次数约束。

结论：避免使用预处理器定义常量。



