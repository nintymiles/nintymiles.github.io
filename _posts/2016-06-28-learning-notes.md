---
layout: post
title:  Learning Notes - NSClassFromString()函数 & SVG animation
---

## Class NSClassFromString (NSString *aClassName)
if (NSClassFromString(@"PHAsset"))... 判断 Photos frmaework 是否已经加载。

> Obtains a class by name.The class object named by aClassName, or nil if no class by that name is currently loaded. If aClassName is nil, returns nil.

## SVG Animation
SVG supports the ability to  ange vector graphics over time. SVG content can be animated in the following ways:

1.  Using SVG's animation elements. SVG document fragments can describe time-based modi cations to the doc- ument's elements. Using the various animation elements, you can de ne motion paths, fade-in or fade-out effects, and objects that grow, shrink, spin or  ange color.
2.  Using the SVG DOM.
3.  SVG has been designed to allow SMIL [SMIL] to use animated or static SVG content as media components.


## **&lt;use&gt;**元素，重用已有的shape

```
<use xlink:href="#a" .../>  //a为shape ID
```
## 伸缩变形

```
<animateTransform attributeName="transform"
                          type="scale"
                          from="0 1" to="1 1"  //x轴从0倍到1倍，y轴从1倍到1呗即y轴不变
                          begin="0s" dur="5s"
                          repeatCount="indefinite"
                />
```
