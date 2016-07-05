---
layout: post
title:  Learning Notes - CALayer 及 Layer Animation
---

## Layer和View的区别
- 联系：Layer是View背后的那个女人。每一个UIView后面都有对应的CALayer，大家看到的在UIView中显示的内容其实是在CALayer中。
- 区别：
    1. View有复杂的、各种组合的布局机制。Layer只有极简单的布局。
    2. View可以响应用户交互。Layer不能响应用户交互。
    3. View中的绘画逻辑有CPU执行。Layer中的绘画直接有GPU执行。
    4. View有丰富的、功能强大的子类。Layer只有很少的几个子类。
    5. View动画属性较少，局限性较大。Layer由于更底层、动画属性更多，所以可以实现出更灵活、更丰富的动画。

## Views vs. layers
A layer is a simple model class that exposes a number of properties to represents some image-based content. Every UIView is backed by a layer, so you can think of layers as the lower-level behind the scenes class behind your content.

A layer is different from a view (with respect to animations) for the following reasons:

- A layer is a model object – it exposes data properties and implements no logic. It has no complex Auto Layout dependencies nor does it handle user interactions.
- It has pre-defined visible traits – these traits are a number of data properties that affect how the contents is rendered on screen, such as border line, border color, position and shadow.
- Finally, Core Animation optimizes the caching of layer contents and fast drawing directly on the GPU.

>UIView is simply a wrapper, providing iOS-specific functionality such as touch handling and high-level interfaces for some of Core Animation’s low-level functionality.

---

The layers cannot handle touch events like UIView can,but here are some features of CALayer that are not exposed by UIView:
- Drop shadows, rounded corners, and colored borders ▪ 3D transforms and positioning
- Nonrectangular bounds
- Alpha masking of content
- Multistep, nonlinear animations

---

## Layer Animation Delegate
With Core Animation, however, you can easily inspect animations that are running on a layer and stop them if you need to.

Furthermore, you can even set a delegate object on your animations and react to animation events. In contrast to the completion block you’ve seen in UIView animations, you can receive delegate callbacks for when an animation begins and ends (or is interrupted).

> One of the tricky parts about UIKit animations and the corresponding closure syntax is that once you create and run a view animation you can’t pause it, stop it, or access it in any way.

### Animation Keys
The key parameter is a string identifier that lets you access and control the animation **after it’s been started**.

### Animation group
Animation groups can let your animations to work **synchronously** and **stay in step** with each other.

```
groupAnimation.animations = [scaleDown, rotate, fade] loginButton.layer.addAnimation(groupAnimation, forKey: nil)
```
### layer animation easing
Easing in layer animations is conceptually the same thing as in view; only the syntax is different.

**CAMediaTimingFunction** has a few pre-defined easing functions, which you can use by name:
- **kCAMediaTimingFunctionLinear** runs the animation with an** equal pace** throughout its whole duration.
- **kCAMediaTimingFunctionEaseIn** alters the animation so it **starts slower** and **finishes at a faster pace**.
- **kCAMediaTimingFunctionEaseOut** produces the opposite effect of kCAMediaTimingFunctionEaseIn; the animations starts out faster and slows down as it finishes.
- **kCAMediaTimingFunctionEaseInEaseOut** slows the animation in the beginning and at the end, but increases the pace during the middle section.

```
groupAnimation.timingFunction = CAMediaTimingFunction( name: kCAMediaTimingFunctionEaseIn) //swift

groupAnimation.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionLinear] //Objective C
```

### Repeating Animation
**repeatCount** lets you repeat your animation a specified number of times.

```
flyLeft.repeatCount = 4
```

Add the following code just after you set the repeatCount:

```
flyLeft.autoreverses = true
```

This will run your animation** in reverse** each time it completes, then run it in forward motion again.

### Animation Spped
You can control the speed of the animation independently of the duration by setting the speed property.

```
flyLeft.speed = 2.0
```

### Layer Geometry
The topic is about how the layer itself is **positioned** and **sized** with respect to its *superlayer* and *siblings*.

- UIView has three primary layout properties: frame, bounds and center
- CALayer has equivalents called frame, bounds, and position

> The frame represents the outer coordinates of the layer (that is, the space it occupies within its superlayer), the bounds property represents the inner coordinates (with {0, 0} typically equating to the top-left corner of the layer, although this is not always the case), and the center and position both represent the location of the anchorPoint relative to the superlayer. 

### AnchorPoint
You can think of the **anchorPoint** as being the handle used to move the layer around.
