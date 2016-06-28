---
layout: post
title:  Learning Notes - NSClassFromString()函数
---

## Class NSClassFromString (NSString *aClassName)
if (NSClassFromString(@"PHAsset"))... 判断 Photos frmaework 是否已经加载。

> Obtains a class by name.The class object named by aClassName, or nil if no class by that name is currently loaded. If aClassName is nil, returns nil.