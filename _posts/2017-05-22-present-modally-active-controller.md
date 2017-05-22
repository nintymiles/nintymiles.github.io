---
layout: post
title: 关于iOS中“present modally an active controller”问题的描述
---

##关于“present modally an active controller”问题的描述
* **问题描述**：当在ViewController中尝试present一个VC时，如果此ViewController同时正被别的VC模态弹出时，会报出如下错误：

```
Application tried to present modally an active controller UITabBarController: 0x83d7f00. 
```
* **问题解决**：杜绝此类情况出现


