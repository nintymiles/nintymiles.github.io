---
layout: post
title:  Learning Notes - CAShapeLayer
---

## What is CAShapeLayer
CAShapeLayer is **a layer subclass** that draws itself using **vector graphics** instead of a bitmap image. You specify attributes such as color and line thickness, *define the desired shape using a **CGPath***, and CAShapeLayer renders it automatically. 

## CAShapeLayer动画在ViewController的注意事项
当在ViewDidLoad中给self.view中的CALayer及子类添加CABasicAnimation效果时，大部分情况动画效果可能不会执行。这里应该有一些冲突的情况。

>CALayer参与UIView的生命周期貌似是稳妥而合适的做法。
