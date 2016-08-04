---
layout: post
title:  Learning Notes - ARC 环境中 Reference Count 理解
---


## ARC 下的内存管理问题
ARC 能够解决 iOS 开发中 90% 的内存管理问题，但是另外还有 10% 内存管理，是需要开发者自己处理的，这主要就是与底层 Core Foundation 对象交互的那部分，底层的 Core Foundation 对象由于不在 ARC 的管理下，所以需要自己维护这些对象的引用计数。同时仍然需要理解引用计数这种内存管理方式的优点和常见问题，特别要注意解决循环引用问题。

问题主要体现在：

1. 过度使用 block 之后，无法解决循环引用问题。
2. 遇到底层 Core Foundation 对象，需要自己手工管理它们的引用计数时，显得一筹莫展。

对于循环引用问题有两种主要的解决办法，一是主动断开循环引用，二是使用弱引用的方式避免循环引用。对于 Core Foundation 对象，由于不在 ARC 管理之下，我们仍然需要延续以前手工管理引用计数的办法。

## 什么是引用计数
引用计数（Reference Count）是一个简单而有效的管理对象生命周期的方式。当我们创建一个新对象的时候，它的引用计数为 1，当有一个新的指针指向这个对象时，我们将其引用计数加 1，当某个指针不再指向这个对象是，我们将其引用计数减 1，当对象的引用计数变为 0 时，说明这个对象不再被任何指针指向了，这个时候我们就可以将对象销毁，回收内存。由于引用计数简单有效，除了 Objective-C 和 Swift 语言外，微软的 COM（Component Object Model ）、C++11（C++11 提供了基于引用计数的智能指针 share_prt）等语言也提供了基于引用计数的内存管理方式。

## 使用弱引用
弱引用虽然持有对象，但是并不增加引用计数，这样就避免了循环引用的产生。在 iOS 开发中，弱引用通常在 delegate 模式中使用。

## 弱引用的实现原理
弱引用的实现原理是这样，系统对于每一个有弱引用的对象，都维护一个表来记录它所有的弱引用的指针地址。这样，当一个对象的引用计数为 0 时，系统就通过这张表，找到所有的弱引用指针，继而把它们都置成 nil。

从这里，我们可以看出，弱引用的使用是有额外的开销的。虽然这个开销很小，但是如果一个地方我们肯定它不需要弱引用的特性，就不应该盲目使用弱引用。

## Core Foundation 对象的内存管理
下面我们就来简单介绍一下对底层 Core Foundation 对象的内存管理。底层的 Core Foundation 对象，在创建时大多以 XxxCreateWithXxx 这样的方式创建，例如：

```
// 创建一个 CFStringRef 对象
CFStringRef str= CFStringCreateWithCString(kCFAllocatorDefault, “hello world", kCFStringEncodingUTF8);

// 创建一个 CTFontRef 对象
CTFontRef fontRef = CTFontCreateWithName((CFStringRef)@"ArialMT", fontSize, NULL);
对于这些对象的引用计数的修改，要相应的使用 CFRetain 和 CFRelease 方法。如下所示：

// 创建一个 CTFontRef 对象
CTFontRef fontRef = CTFontCreateWithName((CFStringRef)@"ArialMT", fontSize, NULL);

// 引用计数加 1
CFRetain(fontRef);
// 引用计数减 1
CFRelease(fontRef);
```

对于 CFRetain 和 CFRelease 两个方法，读者可以直观地认为，这与 Objective-C 对象的 retain 和 release 方法等价。

所以对于底层 Core Foundation 对象，我们只需要延续以前手工管理引用计数的办法即可。

除此之外，还有另外一个问题需要解决。在 ARC 下，我们有时需要将一个 Core Foundation 对象转换成一个 Objective-C 对象，这个时候我们需要告诉编译器，转换过程中的引用计数需要做如何的调整。这就引入了bridge相关的关键字，以下是这些关键字的说明：

`__bridge`: 只做类型转换，不修改相关对象的引用计数，原来的 Core Foundation 对象在不用时，需要调用 `CFRelease` 方法。

`__bridge_retained`：类型转换后，将相关对象的引用计数加 1，原来的 Core Foundation 对象在不用时，需要调用 CFRelease 方法。

`__bridge_transfer`：类型转换后，将该对象的引用计数交给 ARC 管理，Core Foundation 对象在不用时，不再需要调用 CFRelease 方法。

我们根据具体的业务逻辑，合理使用上面的 3 种转换关键字，就可以解决 Core Foundation 对象与 Objective-C 对象相对转换的问题了。

