---
layout: post
title: ObjC - 关于Enumeration
---
## Enumeration的适用范围
### Use Enumerations for States(状态),Options（选项）,and Status Codes（状态码）
Enumberation是一种极为有用的定义可组合命名常量的方式。被广泛使用，遍布系统 `Frameworks`，但是却经常被开发者忽略。 Objective-C从C++11标准受益，最近的版本也支持强类型定义`Enumberation`类型。

Enumeration无非是定位命名常量值的一种方式。使用`Enumeration`就**意味着代码是可读的**，因为每种状态都通过**易读值**被引用。后台支撑`Enumeration`的类型是编译器相关的，要保证有足够的字节来完全表达这个`Enumeration`

Enumeration使用时用typedef别名定义会极大简化使用

The advent of C++ 11 standard brought some changes to enumeration. 新标准的莅临给Enumeration带来了改变。这种变化即有能力指定Enumeration类型的存储变量的底层类型。这么做的好处是可以前置定义Enumeration类型。不指定底层类型，就无法前置`Enumeration`声明。因为编译器不知道要给变量寻址多少空间。

定义某个Enumeration 成员的值也成为可能。这也意味着**之前的**或者**C的机制**是由编译器来选择。

Another reason to use enumeration types is to **define options**, especially when the **options can be combined**. If the enumeration is defined correctly, the options can be combined using the **bitwise OR operator**.

```
enum UIViewAutoresizing { 
	UIViewAutoresizingNone = 0, 
	UIViewAutoresizingFlexibleLeftMargin = 1 << 0, 
	UIViewAutoresizingFlexibleWidth = 1 << 1, 
	UIViewAutoresizingFlexibleRightMargin = 1 << 2, 	UIViewAutoresizingFlexibleTopMargin = 1 << 3, 
	UIViewAutoresizingFlexibleHeight = 1 << 4, 
	UIViewAutoresizingFlexibleBottomMargin = 1 << 5,
}
```

每个有意义的Option占据一个字节位的`1`值（On value）。从而可以使用`OR`运算符进行组合。然后用&符号可以很容易的确认组合中的某个Option。

It’s then possible to determine whether one of the options is set by using the bitwise AND operator:

```
enum UIVewAutoresizing resizing = UIViewAutoresizingFlexibleWidth | UIViewAutoresizingFlexibleHeight; 
if (resizing & UIViewAutoresizingFlexibleWidth) {
	// UIViewAutoresizingFlexibleWidth is set 
}
```
系统提供宏定义支持`Enumeration`定义的向后兼容性。

兼容性宏定义：

```
#if (__cplusplus && __cplusplus >= 201103L && 
(__has_extension(cxx_strong_enums) || __has_feature(objc_fixed_enum)) ) || 
(!__cplusplus && __has_feature(objc_fixed_enum)) 

	#define NS_ENUM(_type, _name)
	enum _name : _type _name; enum _name : _type 
	
	#if (__cplusplus) 
		#define NS_OPTIONS(_type, _name) 
			_type _name; enum : _type 
	#else 
		#define NS_OPTIONS(_type, _name) 
			enum _name : _type _name; enum _name : _type 
	#endif 
	
#else 
	#define NS_ENUM(_type, _name) _type _name; enum 
	#define NS_OPTIONS(_type, _name) _type _name; enum 
#endif
```

