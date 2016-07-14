---
layout: post
title:  Learning Notes - CAShapeLayer 和 UIBezierPath subpath界定
---

## What is CAShapeLayer
CAShapeLayer is **a layer subclass** that draws itself using **vector graphics** instead of a bitmap image. You specify attributes such as color and line thickness, *define the desired shape using a **CGPath***, and CAShapeLayer renders it automatically. 

## CAShapeLayer动画在ViewController的注意事项
当在ViewDidLoad中给self.view中的CALayer及子类添加CABasicAnimation效果时，大部分情况动画效果可能不会执行。这里应该有一些冲突的情况。

>CALayer参与UIView的生命周期貌似是稳妥而合适的做法。

## UIBezierPath的subpath单位界定(Unit Definition)

UIBezierPath以起点，各种线，控制点（非直线），终点构成一个subpath单元如果是闭合形状，则起点和终点可以重合。

UIBezierPath移动到一个点，那么这个点就会被定义为UIBezierPath单元所定义的某种形状的一个起点或者终点。**如果move到一个点，然后画一个线到另一个点，然后再移动到另一个点。那么很明显这个UIBezierPath构成的就是一个直线形状，当然fillColor就会变成无意义的非可见区域了**

A single Bezier path object can contain **any number** of **open** or **closed** subpaths, where each subpath represents a connected series of path segments. *Calling the closePath method closes a subpath by adding a straight line segment from the current point to the first point in the subpath*. **Calling the moveToPoint: method ends the current subpath (without closing it) and *sets the starting point of the next subpath***. The subpaths of a Bezier path object share the same drawing attributes and must be manipulated as a group. To draw subpaths with different attributes, you must put each subpath in its own UIBezierPath object.