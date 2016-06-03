---
layout: post
title: Learning notes
---

## Sketch的Scale Tool
使用Cmd+K激活。The tool isn’t same as resizing,it actually scales every property,size,radius,border,**shadow** and inner **shadow**

> The Sacle Tool even can take effect on Artboard.


## Sketch的Alt键
选中图层后，摁住alt键，会显示选中图层和其他图层的距离(dist)


## MarkDown格式问题
普通正文和有格式正文中间须有空行。比如普通正文和unordered list格式之间，但是xxx list格式及缩进xxx list格式之间不需要空行区隔，如果有空行则实际解析也为空行，但是正常的回车符区隔是少不了的。

> 对于jekyll post尤其要注意这点。


## 开发Mac OS X ／iOS 图片文件归档应用

## Android Activities怎样工作？

- An app is a collection of activities, layouts, and other resources.
- By default, each app runs within its own process.
- You can start an activity in another application by passing an intent with startActivity().（startActivity能力超出预期？）
- When an activity needs to start, Android checks if there’s already a process for that app.
- When Android starts an activity, it calls its onCreate() method.(此处入口)