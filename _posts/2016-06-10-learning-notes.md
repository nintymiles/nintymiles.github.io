---
layout: post
title: Learning Notes - UIBezierPath简单理解
---

## UIBezierPath
The UIBezierPath class lets you define a path consisting of **straight** and **curved line** *segments* and **render that path in your custom views**. You use this class initially to specify just the geometry for your path. Paths can define simple shapes such as rectangles, ovals, and arcs or they can define complex polygons that incorporate a mixture of straight and curved line segments. After defining the shape, you can use additional methods of this class to render the path in the current drawing context.

> UIBezierPath定义包含直线和曲线片段的路径，然后可以在定制的view中渲染这个路径。

## 两个单词

- interpolate:插入 
    > to interpolate a remark 插一句话
- Catmull-Rom:Catmull-Rom插值
    > Little did Catmull and Parke realise that their technology would become the foundation for all special effects in huge Hollywood movies and countless video games. 

- Hermite:艾米插值


## Pop扩展开源项目*MMTweenAnimation*
MMTweenAnimation实际实现为数学曲线公式动画，非简单路径叠加，挺有意思。

