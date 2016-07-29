---
layout: post
title:  Learning Notes - Swift Basic Syntax
---

## Swift extension
With extensions, we can **add** 

- new **properties**, 
- **methods**, 
- **initializers**, 
- and **subscripts**, 
- or make an existing class or structure **conform to a protocol**. 
- One thing to note is that extensions **cannot override the existing functionality**.

可以使用extension规范swift代码风格，将相似部分归纳到一个extension中。

## CoreAnimation的一些协议是使用extension的方式建立在Cocoa之上
这是在swift中core animation相关的委托需要override的原因。

```
extension NSObject {
    
    public func animationDidStart(anim: CAAnimation)
    
    public func animationDidStop(anim: CAAnimation, finished flag: Bool)
}
```


