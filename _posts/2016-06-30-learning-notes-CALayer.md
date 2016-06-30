---
layout: post
title:  Learning Notes - CALayer 
---

## Layer和View的区别
- 联系：Layer是View背后的那个女人。每一个UIView后面都有对应的CALayer，大家看到的在UIView中显示的内容其实是在CALayer中。
- 区别：
    1. View有复杂的、各种组合的布局机制。Layer只有极简单的布局。
    2. View可以响应用户交互。Layer不能响应用户交互。
    3. View中的绘画逻辑有CPU执行。Layer中的绘画直接有GPU执行。
    4. View有丰富的、功能强大的子类。Layer只有很少的几个子类。
    5. View动画属性较少，局限性较大。Layer由于更底层、动画属性更多，所以可以实现出更灵活、更丰富的动画。