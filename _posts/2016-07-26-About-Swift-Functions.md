---
layout: post
title:  Learning Notes - About Swift Function 
---

## Swift 的 Funciton 分类理解
- global function
	全局函数，定义在全局范围。通常可以理解为定义在类同级的函数，可以全局直接访问。等同于C++的全局函数
	
- nested function
	内嵌函数，定义在函数之内的函数

- class function
	类函数，可以理解为类的实例函数，等同于Objective C的instance methods，和java 的 class methods

- class public（class golbal）function
	类公共函数，可以理解为类级别的全局函数，等同于Objective C的class methods，和java class 的 static mehtods
	
	## Cocoa的Objective C函数并没有完全对应swift 函数实现
	比如一些Objective C的类方法没有对应，例如 NSData的`(+)dataWithContentsOfURL:options:error:`,这是因为实现在了init()－类的初始化方法中。这种实现应该是优雅的考虑。

## Git可以识别并支持文件命名改变！！！


