---
layout: post
title:  Learning Notes - Layer Masking
---

## How doest it work for mask layer?
**CALayer** has a property called **mask** that can help with this problem. The mask property is itself a CALayer and has all the same drawing and layout properties of any other layer. It is used in a similar way to a sublayer in that it is positioned relative to its parent (the layer that owns it), but **it does not appear as a normal sublayer**. Instead of being drawn inside the parent, **the mask layer defines the part of the parent layer that is visible**.

The ***color*** of the mask layer is **irrelevant**; all that matters is its silhouette(轮廓). The mask acts like a cookie cutter; the solid part of the mask layer will be “cut out” of its parent layer and kept; **anything else is discarded**

If the mask layer is smaller than the parent layer, only the parts of the parent (or its sub- layers) that **intersect** the mask will be visible. If you are using a mask layer, anything outside of that layer is **implicitly hidden**.

> Mask Layer决定父Layer的可见部分。

> Mask Layer的颜色是无关属性，起作用的只是它的轮廓

> 父Layer正交（正切）Mask Layer的部分可见，任何Mask Layer外部的东西都隐含是不可见的。