---
layout: post
title:  Learning Notes - Layer Masking 和 Scaling Filters
---

## How doest it work for mask layer?
**CALayer** has a property called **mask** that can help with this problem. The mask property is itself a CALayer and has all the same drawing and layout properties of any other layer. It is used in a similar way to a sublayer in that it is positioned relative to its parent (the layer that owns it), but **it does not appear as a normal sublayer**. Instead of being drawn inside the parent, **the mask layer defines the part of the parent layer that is visible**.

The ***color*** of the mask layer is **irrelevant**; all that matters is its silhouette(轮廓). The mask acts like a cookie cutter; the solid part of the mask layer will be “cut out” of its parent layer and kept; **anything else is discarded**

If the mask layer is smaller than the parent layer, only the parts of the parent (or its sub- layers) that **intersect** the mask will be visible. If you are using a mask layer, anything outside of that layer is **implicitly hidden**.

> Mask Layer决定父Layer的可见部分。

> Mask Layer的颜色是无关属性，起作用的只是它的轮廓

> 父Layer正交（正切）Mask Layer的部分可见，任何Mask Layer外部的东西都隐含是不可见的。

## Scaling Filters 缩放过滤算法（尺度滤波器？）
CALayer offers a choice of **three scaling filters** to use when resizing images. These are represented by the following string constants:
- kCAFilterLinear 
- kCAFilterNearest 
- kCAFilterTrilinear

默认的对于minification（缩小）和magnification（放大）的缩放过滤算法都为kCAFilterLinear,This filter uses the **bilinear** filtering algorithm。此算法对于比较大的因素容易产生明显的模糊。

kCAFilterTrilinear大部分情形相似于kCAFilterLinear，但是Trilinear Filter算法通过mip－mapping性能优于billinear。

kCAFilterNearest是最粗暴的选择，速度非常快，但是不模糊图片，在缩小图片时质量显著变坏，放大图片时出现马赛克。但是对于没有斜线的小图，nearest neighboring要优于其它算法。

**CALayer通过minificationFilter和maginificationFilter属性分别设置缩小和放大图片时的算法选择。**