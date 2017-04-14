---
layout: post
title: UITextField输入内容的监听
---
##从内容改变的角度了解
###使用`UITextFieldDelegate`
使用`UITextFieldDelegate`时，对输入内容的处理过滤会极为方便，但是由于是在输入动作的时刻进行拦截，因而从TextField之外的角度审视时，会出现委托方法不够用的时候，导致比如最后一个输入框第一次输入时的有效性响应式验证无法实现（所有输入框都有效时才可以点击提交按钮）（第一个字符输入时，此时ShouldChange委托方法已经调用，但是TextField并没有接受这个输入为内容，因此触发改变有效性验证时无法获取第一个字符的text值）。

### 使用`UITextFieldDidChangeNotification`
使用Notification则从外部观察TextField输入的每个时刻，只要有输入有变化则提醒，并且TextField的text已经获取到改变值。切入时刻不同。


