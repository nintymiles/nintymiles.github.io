---
layout: post
title:  Learning Notes - iPhone 6+ 时代的屏幕适配方案
---
##iPhone 6+ 时代的屏幕适配方案
当启用高分辨率模式支持后（通过LaunchImage，LaunchScreen.xib设置）,如何基于iPhone6（4.7 inch）屏幕进行适配开发？

首选方案是实用autolayout（使用Masonry framework）方案，系统支持从iOS 7+开始。

其次是实用SizeClasses，系统支持从iOS 8+开始。 

> iPhone4、iPhone5、iPhone6 这几个设备的 ppi 都是相同的，默认图片优先是 @2x。iPhone6 Plus 的像素密度更高，默认图片优先是 @3x。

> iPhone6 Plus 有一点和其他设备不同：在 App 内部获得的屏幕分辨率是 1242 * 2208，但设备实际分辨率是 1920 * 1080，这时系统会把整体的显示内容做一个缩放，downscale 到1/1.15

